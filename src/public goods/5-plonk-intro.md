# 从代码中学习 Plonk 协议

写作本文的目的主要是希望从代码的角度理解 Plonk 协议。因为我是开发者，之前读文章遇到公式感觉比较抽象，所以希望有这样的文章，可以从代码的角度来阐述 ZKP 的协议是如何工作的。

这篇文章对应的源代码在[这里](https://github.com/Antalpha-Labs/baby-plonk)，主要实现了 Plonk 协议的核心概念，需要结合[郭宇老师的 Plonk 系列文章](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/README.md)阅读。

## 流程
通过测试 `test.py` 看到验证 Plonk 协议主要分为以下几个部分：
- Setup
- Program
- Assignment
- Generate proof
- Verify

```python=
def prover_test():
    print("Beginning prover test")
    # powers should be 2^n so that we can use roots of unity for FFT
    # and should be bigger than len(coeffs) of polynomial to do KZG commitment
    # the value here is: powers = 4 * group_order
    # which is bigger than the order of quotient polynomial
    group_order = 8
    powers = group_order * 4
    setup = Setup.generate_srs(powers)

    program = Program(["e public", "c <== a * b", "e <== c * d"], group_order)
    assignments = {"a": 3, "b": 4, "c": 12, "d": 5, "e": 60}
    prover = Prover(setup, program)
    proof = prover.prove(assignments)
    print("Prover test success")
    return setup, proof, group_order

def verifier_test(setup, proof, group_order):
    print("Beginning verifier test")
    program = Program(["e public", "c <== a * b", "e <== c * d"], group_order)
    public = [60]
    vk = setup.verification_key(program.common_preprocessed_input())
    assert vk.verify_proof(group_order, proof, public)
    print("Verifier test success")

```

整个协议的过程：
1. 给定一个计算/电路/程序:
```
a * b = c
c * d = e
其中 e 是公开值
```
2. prover 选择特定的一组值 witness = (a, b, c, d, e)，这组值满足上面的约束条件
3. 在保持 witness 不公开的前提下，prover 生成一个证明 proof, 可以证明 prover 知道 witness
4. verifier 验证 proof 的真实性

下面我们通过代码依次看看每个步骤都做了什么。

## Setup
```python=
@dataclass
class Setup(object):
    #   ([1]₁, [x]₁, ..., [x^{d-1}]₁)
    # = ( G,    xG,  ...,  x^{d-1}G ), where G is a generator of G_1
    powers_of_x: list[G1Point]
    # [x]₂ = xH, where H is a generator of G_2
    X2: G2Point

    @classmethod
    def generate_srs(cls, powers: int):
        print("Start to generate structured reference string")
        # tau is a random number whatever you choose
        tau = 218313819403157342856071133

        # Initialize powers_of_x with 0 values
        powers_of_x = [0] * powers
        # powers_of_x[0] =  b.G1 * tau**0 = b.G1
        # powers_of_x[1] =  b.G1 * tau**1 = powers_of_x[0] * tau
        # powers_of_x[2] =  b.G1 * tau**2 = powers_of_x[1] * tau
        # ...
        # powers_of_x[i] =  b.G1 * tau**i = powers_of_x[i - 1] * tau
        powers_of_x[0] = b.G1

        for i in range(powers):
            if i > 0:
                powers_of_x[i] = b.multiply(powers_of_x[i - 1], tau)

        assert b.is_on_curve(powers_of_x[1], b.b)
        print("Generated G1 side, X^1 point: {}".format(powers_of_x[1]))

        X2 = b.multiply(b.G2, tau)
        assert b.is_on_curve(X2, b.b2)
        print("Generated G2 side, X^1 point: {}".format(X2))

        assert b.pairing(b.G2, powers_of_x[1]) == b.pairing(X2, b.G1)
        print("X^1 points checked consistent")
        print("Finished to generate structured reference string")

        return cls(powers_of_x, X2)

    # Encodes the KZG commitment that evaluates to the given values in the group
    def commit(self, values: Polynomial) -> G1Point:
        if (values.basis == Basis.LAGRANGE):
            # inverse FFT from Lagrange basis to monomial basis
            coeffs = values.ifft().values
        elif (values.basis == Basis.MONOMIAL):
            coeffs = values.values
        if len(coeffs) > len(self.powers_of_x):
            raise Exception("Not enough powers in setup")
        return ec_lincomb([(s, x) for s, x in zip(self.powers_of_x, coeffs)])

    # Generate the verification key for this program with the given setup
    def verification_key(self, pk: CommonPreprocessedInput) -> VerificationKey:
        return VerificationKey(
            pk.group_order,
            self.commit(pk.QM),
            self.commit(pk.QL),
            self.commit(pk.QR),
            self.commit(pk.QO),
            self.commit(pk.QC),
            self.commit(pk.S1),
            self.commit(pk.S2),
            self.commit(pk.S3),
            self.X2,
            Scalar.root_of_unity(pk.group_order),
        )
    
```
这里有几个函数，第一个函数 `generate_srs` 用于生成 structured reference string(SRS)，用于给多项式在群上生成 KZG commitment。基本流程：
1. 选择一个随机数作为 tau 的值
2. 从椭圆曲线上获得两个生成元 G1 和 G2，有现成的函数库可以拿到
3. 生成所需要的 SRS 值
4. 最后对生成的值进行验证

第二个函数 `commit`，就是实际用来生成 KZG commitment 的函数

第三个函数 `verification_key` 用来给 verifier 生成 verification key，用来验证 proof

## Program
```python
program = Program(["e public", "c <== a * b", "e <== c * d"], group_order)
```
Program 类的目标是 Arithmetization，将某种计算转换成数学表示。这里的`计算`指的是一段电路，`数学表示`指的是多项式。

在 Plonk 中，可以用八个多项式来表示这个 Program: QL, QR, QM, QO, QC, S1, S2, S3。所以 Program 类的主要目标就是处理上面电路的字符串的表示，最终得到这八个多项式。这八个多项式是公开的，prover 和 verifier 都可以得到这个信息。

经过一定的处理，prover 得到 prover key(pk)。
```python=
@dataclass
class CommonPreprocessedInput:
    """Common preprocessed input"""

    group_order: int
    # q_M(X) multiplication selector polynomial
    QM: Polynomial
    # q_L(X) left selector polynomial
    QL: Polynomial
    # q_R(X) right selector polynomial
    QR: Polynomial
    # q_O(X) output selector polynomial
    QO: Polynomial
    # q_C(X) constants selector polynomial
    QC: Polynomial
    # S_σ1(X) first permutation polynomial S_σ1(X)
    S1: Polynomial
    # S_σ2(X) second permutation polynomial S_σ2(X)
    S2: Polynomial
    # S_σ3(X) third permutation polynomial S_σ3(X)
    S3: Polynomial

```
verifier 得到 verification key，在上面的 `Setup` 步骤中也提到了。
```python=
    # Generate the verification key for this program with the given setup
    def verification_key(self, pk: CommonPreprocessedInput) -> VerificationKey:
        return VerificationKey(
            pk.group_order,
            self.commit(pk.QM),
            self.commit(pk.QL),
            self.commit(pk.QR),
            self.commit(pk.QO),
            self.commit(pk.QC),
            self.commit(pk.S1),
            self.commit(pk.S2),
            self.commit(pk.S3),
            self.X2,
            Scalar.root_of_unity(pk.group_order),
        )
```
和 prover 不一样的是，为什么中间 8 个值要用 commitment 的形式发给 verifier 呢？这是因为 Plonk 协议为了保证 verifier 端验证的计算复杂度尽量低，所以没有给出原始的多项式，而只给出了 KZG 承诺的值，后面会看到，verifier 通过 pairing 验证就可以保证这些承诺值和原始的多项式是一一对应的，prover 欺骗不了 verifier，这样既保证的正确性，也保证了 verifier 验证的简单性。

## Assignment
```python
assignments = {"a": 3, "b": 4, "c": 12, "d": 5, "e": 60}
```

Assignment 是对电路中引线的赋值，也叫 witness 或者 private input。这些值只有 prover 知道，对 verifier 是保密的。prover 最终要向 verifier 提供证明，保证将这些值输入到 program 中能得到指定的结果。

## Generate proof
```python
proof = prover.prove(assignments)
```
这里是协议的重点，分为五轮。


### 主体逻辑：
```python=
    def prove(self, witness: dict[Optional[str], int]) -> Proof:
        # Initialise Fiat-Shamir transcript
        transcript = Transcript(b"plonk")

        # Collect fixed and public information
        # FIXME: Hash pk and PI into transcript
        public_vars = self.program.get_public_assignments()
        PI = Polynomial(
            [Scalar(-witness[v]) for v in public_vars]
            + [Scalar(0) for _ in range(self.group_order - len(public_vars))],
            Basis.LAGRANGE,
        )
        self.PI = PI

        # Round 1
        msg_1 = self.round_1(witness)
        self.beta, self.gamma = transcript.round_1(msg_1)

        # Round 2
        msg_2 = self.round_2()
        self.alpha, self.fft_cofactor = transcript.round_2(msg_2)

        # Round 3
        msg_3 = self.round_3()
        self.zeta = transcript.round_3(msg_3)

        # Round 4
        msg_4 = self.round_4()
        self.v = transcript.round_4(msg_4)

        # Round 5
        msg_5 = self.round_5()

        return Proof(msg_1, msg_2, msg_3, msg_4, msg_5)

```
通过 5 轮的计算会生成必要的 proof，这些 proof 之后交给 verifier 进行验证，如果通过，则整个协议完成。

### Round 0: 初始化：
```python=
    def __init__(self, setup: Setup, program: Program):
        self.group_order = program.group_order
        self.setup = setup
        self.program = program
        self.pk = program.common_preprocessed_input()

```


### Round 1: 生成对 witness/assignments 多项式的承诺
这个过程和相关知识可以参考 [理解 PLONK（一）：Plonkish Arithmetization
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-arithmetization.md) 和 [理解 PLONK（二）：多项式编码
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-lagrange-basis.md)。多项式承诺相关知识可以参考 [理解 Plonk（五）：多项式承诺
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-polycom.mdf)

大体流程:
1. 根据 group_order 初始化 A, B, C 这三个 witness 多项式的点值 A_values，B_values，C_values，这些点值用于后面生成多项式，也就是生成的 `Polynomial` 类
2. 依次读取 program 中引线的值，将左引线的值 L 添加到 A_values, 右引线的值添加到 B_values 中，输出引线的值添加到 C_values 中
3. 通过 `Polynomial` 类生成 A，B，C 多项式
4. 生成 A，B，C 多项式的 KZG 承诺
5. 验证门约束等式是否成立

```python=
    def round_1(
        self,
        witness: dict[Optional[str], int],
    ) -> Message1:
        program = self.program
        setup = self.setup
        group_order = self.group_order

        if None not in witness:
            witness[None] = 0

        # 1. 根据 group_order 初始化 A, B, C 多项式的点值
        # A_values，B_values，C_values，这些点值用于后面
        # 生成多项式，也就是 `Polynomial` 类
        # Compute wire assignments
        A_values = [Scalar(0) for _ in range(group_order)]
        B_values = [Scalar(0) for _ in range(group_order)]
        C_values = [Scalar(0) for _ in range(group_order)]

        # 2. 依次读取 program 中引线的值，将左引线的值 L 添加到 A_values, 
        # 右引线的值添加到 B_values 中，输出引线的值添加到 C_values 中
        for i, gate_wires in enumerate(program.wires()):
            A_values[i] = Scalar(witness[gate_wires.L])
            B_values[i] = Scalar(witness[gate_wires.R])
            C_values[i] = Scalar(witness[gate_wires.O])

        # 3. 通过 `Polynomial` 类生成 A，B，C 多项式
        self.A = Polynomial(A_values, Basis.LAGRANGE)
        self.B = Polynomial(B_values, Basis.LAGRANGE)
        self.C = Polynomial(C_values, Basis.LAGRANGE)

        # 4. 生成 A，B，C 多项式的 KZG 承诺
        a_1 = setup.commit(self.A)
        b_1 = setup.commit(self.B)
        c_1 = setup.commit(self.C)

        # 5. 验证门约束等式是否成立
        # Sanity check that witness fulfils gate constraints
        assert (
            self.A * self.pk.QL
            + self.B * self.pk.QR
            + self.A * self.B * self.pk.QM
            + self.C * self.pk.QO
            + self.PI
            + self.pk.QC
            == Polynomial([Scalar(0)] * group_order, Basis.LAGRANGE)
        )

        return Message1(a_1, b_1, c_1)
```


### Round 2: 生成 Permutation Accumulator 多项式 Z 的 KZG 承诺
参考文章 [理解 PLONK（三）：置换证明
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-permutation.md)

要给 `Z` 生成承诺，首先要构造`Z`，然后可以直接对多项式生成 KZG 承诺。

大体流程：
1. 初始化`Z` 的点值数组 `Z_values` 第一个值为 1
2. 在 group_order 内，依次用上一次的值乘以当前的累乘因子，获得当前 `Z_values` 的值
3. 确保最后一项为 1（具体原理请看上面的文章）
4. 检查生成值的有效性
5. 用 Lagrange 形式构造多项式
6. 生成 KZG 承诺

```python=
    def round_2(self) -> Message2:
        group_order = self.group_order
        setup = self.setup

        Z_values = [Scalar(1)]
        roots_of_unity = Scalar.roots_of_unity(group_order)
        for i in range(group_order):
            Z_values.append(
                Z_values[-1]
                * self.rlc(self.A.values[i], roots_of_unity[i])
                * self.rlc(self.B.values[i], 2 * roots_of_unity[i])
                * self.rlc(self.C.values[i], 3 * roots_of_unity[i])
                / self.rlc(self.A.values[i], self.pk.S1.values[i])
                / self.rlc(self.B.values[i], self.pk.S2.values[i])
                / self.rlc(self.C.values[i], self.pk.S3.values[i])
            )
        assert Z_values.pop() == 1

        # Sanity-check that Z was computed correctly
        for i in range(group_order):
            assert (
                self.rlc(self.A.values[i], roots_of_unity[i])
                * self.rlc(self.B.values[i], 2 * roots_of_unity[i])
                * self.rlc(self.C.values[i], 3 * roots_of_unity[i])
            ) * Z_values[i] - (
                self.rlc(self.A.values[i], self.pk.S1.values[i])
                * self.rlc(self.B.values[i], self.pk.S2.values[i])
                * self.rlc(self.C.values[i], self.pk.S3.values[i])
            ) * Z_values[
                (i + 1) % group_order
            ] == 0

        Z = Polynomial(Z_values, Basis.LAGRANGE)
        z_1 = setup.commit(Z)
        print("Permutation accumulator polynomial successfully generated")

        self.Z = Z
        return Message2(z_1)

```

其中 `rlc` 的定义：
```python=
    def rlc(self, term_1, term_2):
        return term_1 + term_2 * self.beta + self.gamma

```

### Round 3: 生成商多项式的承诺
相关知识可以参考文章 [理解 PLONK（四）：算术约束与拷贝约束
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-constraints.md)

大体流程:
1. 构造消失多项式(Vanishing Polynomial): ZH_coeff
2. 构造电路的门约束多项式: `gate_constraints_coeff`
3. 构造 Copy Constraints 的多项式: `permutation_grand_product`
4. 构造 `Copy Constraints 第一个值为 1` 这个约束的多项式: `permutation_first_row_coeff`
5. 求出商多项式 `quotient polynomial`: T_coeff
6. 计算商多项式的 KZG 承诺
```python=
    def round_3(self) -> Message3:
        group_order = self.group_order
        setup = self.setup

        # Compute the quotient polynomial

        alpha = self.alpha

        roots_of_unity = Scalar.roots_of_unity(group_order)

        A_coeff, B_coeff, C_coeff, S1_coeff, S2_coeff, S3_coeff, Z_coeff, QL_coeff, QR_coeff, QM_coeff, QO_coeff, QC_coeff, PI_coeff = (
            x.ifft()
            for x in (
                self.A,
                self.B,
                self.C,
                self.pk.S1,
                self.pk.S2,
                self.pk.S3,
                self.Z,
                self.pk.QL,
                self.pk.QR,
                self.pk.QM,
                self.pk.QO,
                self.pk.QC,
                self.PI,
            )
        )

        L0_coeff = (
            Polynomial([Scalar(1)] + [Scalar(0)] * (group_order - 1), Basis.LAGRANGE)
        ).ifft()

        # x^8 - 1 coeffs are [-1, 0, 0, 0, 0, 0, 0, 0, 1]
        # which needs 9 points(n + 1) to determine the polynomial
        ZH_array = [Scalar(-1)] + [Scalar(0)] * (group_order - 1) + [Scalar(1)]
        ZH_coeff = Polynomial(ZH_array, Basis.MONOMIAL)

        gate_constraints_coeff = (
            A_coeff * QL_coeff
            + B_coeff * QR_coeff
            + A_coeff * B_coeff * QM_coeff
            + C_coeff * QO_coeff
            + PI_coeff
            + QC_coeff
        )

        normal_roots = Polynomial(
            roots_of_unity, Basis.LAGRANGE
        )

        roots_coeff = normal_roots.ifft()
        # z * w
        ZW = self.Z.shift(1)
        ZW_coeff = ZW.ifft()

        for i in range(group_order):
            assert (
                self.rlc(self.A.values[i], roots_of_unity[i])
                * self.rlc(self.B.values[i], 2 * roots_of_unity[i])
                * self.rlc(self.C.values[i], 3 * roots_of_unity[i])
            ) * self.Z.values[i] - (
                self.rlc(self.A.values[i], self.pk.S1.values[i])
                * self.rlc(self.B.values[i], self.pk.S2.values[i])
                * self.rlc(self.C.values[i], self.pk.S3.values[i])
            ) * ZW.values[
                i % group_order
            ] == 0

        permutation_grand_product_coeff = (
            (
                self.rlc(A_coeff, roots_coeff)
                * self.rlc(B_coeff, roots_coeff * Scalar(2))
                * self.rlc(C_coeff, roots_coeff * Scalar(3))
            )
            * Z_coeff
            - (
                self.rlc(A_coeff, S1_coeff)
                * self.rlc(B_coeff, S2_coeff)
                * self.rlc(C_coeff, S3_coeff)
            )
            * ZW_coeff
        )

        permutation_first_row_coeff = (Z_coeff - Scalar(1)) * L0_coeff

        all_constraints = (
            gate_constraints_coeff
            + permutation_grand_product_coeff * alpha
            + permutation_first_row_coeff * alpha**2
        )

        # quotient polynomial
        T_coeff = all_constraints / ZH_coeff

        print("Generated the quotient polynomial")

        W_t = setup.commit(T_coeff)

        self.A_coeff = A_coeff
        self.B_coeff = B_coeff
        self.C_coeff = C_coeff
        self.S1_coeff = S1_coeff
        self.S2_coeff = S2_coeff
        self.S3_coeff = S3_coeff
        self.Z_coeff = Z_coeff
        self.ZW_coeff = ZW_coeff
        self.QL_coeff = QL_coeff
        self.QR_coeff = QR_coeff
        self.QM_coeff = QM_coeff
        self.QO_coeff = QO_coeff
        self.QC_coeff = QC_coeff
        self.PI_coeff = PI_coeff
        self.T_coeff = T_coeff

        return Message3(W_t)
```

### Round 4: 对各个多项式在一个随机的 zeta 点求值
相关知识可以参考文章 [理解 PLONK（四）：算术约束与拷贝约束
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-constraints.md)

这一步比较简单，对各个多项式在一个随机的 zeta 点求值。这里有一个知识点，就是如何获取 zeta 这个随机值，使用的方法叫做Fiat-Shamir 变换，可以将一个需要 prover 和 verifier 进行交互的证明转化成不需要交互的证明，简单介绍可以参考 [这篇文章](https://github.com/sec-bit/learning-zkp/blob/3bff37c46c7b4f447b0bb9304d1ebc039a582c5f/zkp-intro/4/zkp-rom.md#fiat-shamir-%E5%8F%98%E6%8D%A2--%E4%BB%8E-public-coin-%E5%88%B0-nizk)，代码可以参考 [这里](https://github.com/Antalpha-Labs/baby-plonk/blob/main/transcript.py)。

```python=
    def round_4(self) -> Message4:
        group_order = self.group_order
        zeta = self.zeta

        a_eval = self.A_coeff.coeff_eval(zeta)
        b_eval = self.B_coeff.coeff_eval(zeta)
        c_eval = self.C_coeff.coeff_eval(zeta)
        s1_eval = self.S1_coeff.coeff_eval(zeta)
        s2_eval = self.S2_coeff.coeff_eval(zeta)
        s3_eval = self.S3_coeff.coeff_eval(zeta)
        root_of_unity = Scalar.root_of_unity(group_order)
        z_eval = self.Z_coeff.coeff_eval(zeta)
        zw_eval = self.Z_coeff.coeff_eval(zeta * root_of_unity)
        ql_eval = self.QL_coeff.coeff_eval(zeta)
        qr_eval = self.QR_coeff.coeff_eval(zeta)
        qm_eval = self.QM_coeff.coeff_eval(zeta)
        qo_eval = self.QO_coeff.coeff_eval(zeta)
        qc_eval = self.QC_coeff.coeff_eval(zeta)
        t_eval = self.T_coeff.coeff_eval(zeta)

        self.a_eval = a_eval
        self.b_eval = b_eval
        self.c_eval = c_eval
        self.ql_eval = ql_eval
        self.qr_eval = qr_eval
        self.qm_eval = qm_eval
        self.qo_eval = qo_eval
        self.qc_eval = qc_eval
        self.s1_eval = s1_eval
        self.s2_eval = s2_eval
        self.s3_eval = s3_eval
        self.z_eval = z_eval
        self.zw_eval = zw_eval
        self.t_eval = t_eval

        return Message4(
            a_eval,
            b_eval,
            c_eval,
            ql_eval,
            qr_eval,
            qm_eval,
            qo_eval,
            qc_eval,
            s1_eval,
            s2_eval,
            s3_eval,
            z_eval,
            zw_eval,
            t_eval
        )

```
`coeff_eval` 是一个多项式求值的函数。具体实现可以看[这里](https://github.com/Antalpha-Labs/baby-plonk/blob/main/poly.py)。

### Round 5: 对每个多项式生成 KZG 承诺中需要的两个承诺
相关知识可以参考文章 [理解 PLONK（四）：算术约束与拷贝约束
](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-constraints.md)。

这一步也比较简单，生成所需的 KZG 承诺，为 verifier 进行 verify 做准备。

```python=
    def round_5(self) -> Message5:
        W_a, W_a_quot = self.generate_commitment(self.A_coeff, self.a_eval)
        W_b, W_b_quot = self.generate_commitment(self.B_coeff, self.b_eval)
        W_c, W_c_quot = self.generate_commitment(self.C_coeff, self.c_eval)
        W_ql, W_ql_quot = self.generate_commitment(self.QL_coeff, self.ql_eval)
        W_qr, W_qr_quot = self.generate_commitment(self.QR_coeff, self.qr_eval)
        W_qm, W_qm_quot = self.generate_commitment(self.QM_coeff, self.qm_eval)
        W_qo, W_qo_quot = self.generate_commitment(self.QO_coeff, self.qo_eval)
        W_qc, W_qc_quot = self.generate_commitment(self.QC_coeff, self.qc_eval)
        W_s1, W_s1_quot = self.generate_commitment(self.S1_coeff, self.s1_eval)
        W_s2, W_s2_quot = self.generate_commitment(self.S2_coeff, self.s2_eval)
        W_s3, W_s3_quot = self.generate_commitment(self.S3_coeff, self.s3_eval)
        W_z, W_z_quot = self.generate_commitment(self.Z_coeff, self.z_eval)
        W_zw, W_zw_quot = self.generate_commitment(self.ZW_coeff, self.zw_eval)
        W_t, W_t_quot = self.generate_commitment(self.T_coeff, self.t_eval)

        print("Generated final quotient witness polynomials")
        return Message5(
            W_a, W_a_quot,
            W_b, W_b_quot,
            W_c, W_c_quot,
            W_ql, W_ql_quot,
            W_qr, W_qr_quot,
            W_qm, W_qm_quot,
            W_qo, W_qo_quot,
            W_qc, W_qc_quot,
            W_s1, W_s1_quot,
            W_s2, W_s2_quot,
            W_s3, W_s3_quot,
            W_z, W_z_quot,
            W_zw, W_zw_quot,
            W_t, W_t_quot,
        )
```

生成承诺的代码:
```python=

    def generate_commitment(self, coeff: Polynomial, eval: Scalar):
        setup = self.setup
        zeta = self.zeta
        # Polynomial for (X - zeta)
        ZH_zeta_coeff = Polynomial([-zeta, Scalar(1)], Basis.MONOMIAL)
        quot_coeff = (coeff - eval) / ZH_zeta_coeff
        # witness for polynomial itself
        w = setup.commit(coeff)
        # witness for quotient polynomial
        w_quot = setup.commit(quot_coeff)
        return w, w_quot

```


## Verify
再回顾一下测试中的代码是这样进行验证的:
```python=
def verifier_test(setup, proof, group_order):
    print("Beginning verifier test")
    program = Program(["e public", "c <== a * b", "e <== c * d"], group_order)
    public = [60]
    vk = setup.verification_key(program.common_preprocessed_input())
    assert vk.verify_proof(group_order, proof, public)
    print("Verifier test success")

```

`VerificationKey` 的代码在[这里](https://github.com/Antalpha-Labs/baby-plonk/blob/main/verifier.py)，最核心的方法是 `verify_proof`

verifier 主要做两件事情:
1. 验证 KZG 承诺，保证多项式是和所承诺的一致
2. 验证最终组合出来的多项式求值的相等性

```python=
    def verify_proof(self, group_order: int, pf, public=[]) -> bool:
        # 4. Compute challenges
        beta, gamma, alpha, zeta, v, u = self.compute_challenges(pf)
        proof = pf.flatten()

        # 5. Compute zero polynomial evaluation Z_H(ζ) = ζ^n - 1
        ZH_ev = zeta**group_order - 1

        # 6. Compute Lagrange polynomial evaluation L_0(ζ)
        L0_ev = ZH_ev / (group_order * (zeta - 1))

        # 7. Compute public input polynomial evaluation PI(ζ).
        PI = Polynomial(
            [Scalar(-x) for x in public]
            + [Scalar(0) for _ in range(group_order - len(public))],
            Basis.LAGRANGE,
        )
        PI_ev = PI.barycentric_eval(zeta)

        # verify KZG10 commitment
        self.verify_commitment(proof, proof["W_a"], "W_a_quot", "a_eval", zeta)
        self.verify_commitment(proof, proof["W_b"], "W_b_quot", "b_eval", zeta)
        self.verify_commitment(proof, proof["W_c"], "W_c_quot", "c_eval", zeta)
        self.verify_commitment(proof, proof["W_z"], "W_z_quot", "z_eval", zeta)
        self.verify_commitment(proof, proof["W_zw"], "W_zw_quot", "zw_eval", zeta)
        self.verify_commitment(proof, proof["W_t"], "W_t_quot", "t_eval", zeta)
        self.verify_commitment(proof, self.Ql, "W_ql_quot", "ql_eval", zeta)
        self.verify_commitment(proof, self.Qr, "W_qr_quot", "qr_eval", zeta)
        self.verify_commitment(proof, self.Qm, "W_qm_quot", "qm_eval", zeta)
        self.verify_commitment(proof, self.Qo, "W_qo_quot", "qo_eval", zeta)
        self.verify_commitment(proof, self.Qc, "W_qc_quot", "qc_eval", zeta)
        self.verify_commitment(proof, self.S1, "W_s1_quot", "s1_eval", zeta)
        self.verify_commitment(proof, self.S2, "W_s2_quot", "s2_eval", zeta)
        self.verify_commitment(proof, self.S3, "W_s3_quot", "s3_eval", zeta)

        a_eval = proof["a_eval"]
        b_eval = proof["b_eval"]
        c_eval = proof["c_eval"]
        ql_eval = proof["ql_eval"]
        qr_eval = proof["qr_eval"]
        qm_eval = proof["qm_eval"]
        qo_eval = proof["qo_eval"]
        qc_eval = proof["qc_eval"]
        s1_eval = proof["s1_eval"]
        s2_eval = proof["s2_eval"]
        s3_eval = proof["s3_eval"]
        z_eval = proof["z_eval"]
        zw_eval = proof["zw_eval"]
        t_eval = proof["t_eval"]

        f_eval = (
            (a_eval + beta * zeta + gamma)
            * (b_eval + beta * zeta * 2 + gamma)
            * (c_eval + beta * zeta * 3 + gamma)
        )
        g_eval = (
            (a_eval + beta * s1_eval + gamma)
            * (b_eval + beta * s2_eval + gamma)
            * (c_eval + beta * s3_eval + gamma)
        )

        gate_constraints_eval = (
            ql_eval * a_eval
            + qr_eval * b_eval
            + qm_eval * a_eval * b_eval
            + qo_eval * c_eval
            + qc_eval
            + PI_ev
        )

        permutation_grand_product_eval = z_eval * f_eval - zw_eval * g_eval

        permutation_first_row_eval = L0_ev * (z_eval - 1)

        left = (
            gate_constraints_eval
            + alpha * permutation_grand_product_eval
            +  alpha ** 2 * permutation_first_row_eval
        )

        right = t_eval * ZH_ev

        assert left == right

        print("Done equation check for all constraints")
        return True

    # Compute challenges (should be same as those computed by prover)
    def compute_challenges(
        self, proof
    ) -> tuple[Scalar, Scalar, Scalar, Scalar, Scalar, Scalar]:
        transcript = Transcript(b"plonk")
        beta, gamma = transcript.round_1(proof.msg_1)
        alpha, _fft_cofactor = transcript.round_2(proof.msg_2)
        zeta = transcript.round_3(proof.msg_3)
        v = transcript.round_4(proof.msg_4)
        u = transcript.round_5(proof.msg_5)

        return beta, gamma, alpha, zeta, v, u

    def verify_commitment(self, proof, W, W_quot_key, eval_key, zeta):
        W_quot = proof[W_quot_key]
        eval = proof[eval_key]
        ec_comb = ec_lincomb(
            [
                (W, 1),
                (W_quot, zeta),
                (b.G1, -eval),
            ]
        )

        assert b.pairing(self.X_2, W_quot) == b.pairing(b.G2, ec_comb)
        print(f"Done KZG10 commitment check for {eval_key} polynomial")

```

## 最后
以上就是 Plonk 协议的代码讲解，接下来建议读者亲自运行一下[这个代码](https://github.com/Antalpha-Labs/baby-plonk)，打印其中一些值看看，这样会对协议的了解更加深刻。
