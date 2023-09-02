<!-- toc -->
# 深入分析SuperNova及其ROM实现
针对于为有限状态机上程序执行的正确性的问题，Nova是集大成者，作者Setty提出了一种基于Folding的递归证明系统。但是Nova要求在迭代中使用相同的业务电路，可以理解为仅支持单个指令。SuperNova则对其进行了拓展，在每步迭代中可以运行不同的指令(这一问题定义为NIVC, non-uniform IVC)，因此可以把Nova看作是只支持一个指令的NIVC解决方案。之前采用全局电路的方法其开销与所有指令构成的电路规模有关，Supernova最大的创新则是其证明开销只与当前步执行的指令有关，并且产生的overhead是常数。

本文首先介绍NIVC问题的定义，以及Supernova的基本思路，最后针对于文中没有给出具体说明的电路选择器，详细介绍了ROM模型的实现思路。

## NIVC定义
![image](https://github.com/zkp-co-learning/zkp-co-learn/assets/13568446/f9bfb895-4c3c-4ff4-8b00-a7cb6867abbb)
NIVC是对IVC的泛化，在每步增量计算中，Prover可以证明满足一些列relation中的一种relation，所以他可以支持每步使用不同的电路。
首先定义NIVC要证明的电路形式，假设存在$`l`$个多项式时间可计算的函数$`{F_1, F_2,...,F_l}`$ (可以把他们看作是执行一些列不同指令的电路)，他们满足:$`z_{i+1} = F_{ϕ(w_i, z_i)}(w_i,z_i)`$，其中$`ϕ`$为选择器，其根据当前witness $`w_i`$和公共输出$`z_i`$选择其中第$`j`$个函数$`F_j`$，即输出$`j, 1≤j ≤l`$。

Prover则是要生成proof，其可以证明对于n步迭代中产生的一系列$`\{ω_0,...,w_{n-1} \}$和$\{z_0,...,z_{n-1} \}`$均满足$`z_{i+1} = F_{ϕ(w_i, z_i)}(w_i,z_i)`$。可将其形式化表述为$`P(pk,(i, z_0, z_i), ω_i , Π_i) → Π_{i+1}`$， 其中P为Prover，pk为prover key，$`Π`$为proof。与IVC类似，NIVC要求Prover在任意步的证明开销与之前调用的指令无关，否则会导致电路规模无限增大；更进一步要求Prover的开销只和当前步运行的电路规模有关，否则就蜕化成了用一个包含所有函数电路构成的IVC。

## Supernova证明系统
对于上述证明问题，Supernova采用了类似与Nova的folding scheme，总体来说其也是先构建一个Augumented函数$`F_j'`$，通过证明存在满足$`F_j'`$的witness，来证明业务电路F以及每次迭代更新proof的正确性。$`F_j'`$每次需要把relaxed R1CS实例(U)和r1cs实例(u)进行fold，而且folding scheme要求两种实例的结构是一样的，但是NIVC中$`F'`$函数有多个，不能简单地fold。因此，SuperNova中在第$`i`$步$`F_j'`$会输入一系列的$`U_i`$，其中$`U_i[j]`$代表$`F_j'`$从0到$`i-1`$步被正确执行，这样只需要验证所有的$`U_i`$是否满足约束就可以验证所有函数从0到$`i-1`$步被正确执行；此外$`F_j'`$还会输入一个实例u，用来证明第$`i`$步也被正确执行。
![image](https://github.com/zkp-co-learning/zkp-co-learn/assets/13568446/29cfd5e8-00a8-455d-bf9b-64a367f35683)
对于Augumented函数，相对于Nova的不同点在于，在第$`i`$步只折叠第$`pc_i = \phi(w_i, z_i)`$个实例，为了确保执行的是$`pc_i`$，需要将$`pc_i`$也作为公共输入放入$`z_i`$中来进行检验。

Supernova证明系统的核心构造为：
![image](https://github.com/zkp-co-learning/zkp-co-learn/assets/13568446/0ebe1192-683e-4192-822f-29876f1d338f)
![image](https://github.com/zkp-co-learning/zkp-co-learn/assets/13568446/c4c9fb8e-c9c5-4909-9e84-71870e04f71c)

> 说明：实际上Relaxed R1CS(Az◦Bz = uCz + E)和R1CS(Az◦Bz=Cz)中的A,B,C是一致（这些值由业务电路+Folding相关的约束生成），只是具体的z不一样。

需要注意的是上述证明系统并**没有明确约束第$`i`$步具体选择哪个电路**，因此如果需要确定性生成相应的选择器，还需增加选择器电路。
然而论文中没有给出选择器的具体实现，下面参考PSE一位成员给出的一种[电路序列固定的Supernova实现](https://github.com/microsoft/Nova/pull/204)，进一步给出具体实现细节。
## ROM machine based Supernova
Rom( read-only memory)模型将所有的电路看作电路序列，该序列共有$`n`$个电路，其中不同的电路共有$`l`$个，并将所有的电路直接写死在Supernova的公共输入$`z[1...l]`$中，每迭代一步$`pc+1`$, $`pc`$则放入$`z[0]`$中。在第$`i`$步时，读取$`pc_i=z[0]`$，并选取$`z[pc_i]`$对应的电路。
比如共有2个不同的电路$`Circuit_1, Circuit_2`$，ROM构成的电路序列为$`Circuit_1, Circuit_2,Circuit_1, Circuit_1...`$

那么在Supernova论文给出的证明系统之上，还需保证：
1. 在$`i`$步fold的是第$`i`$个电路；
2. 在第$`i`$步选择是第$`rom[i]`$电路（注意这点在supernova中没有要求)

对于第1个问题，主要通过构造一个条件选择电路，具体电路如下：
```
    // select target when index match last_augmented_circuit_index, other left as empty

    let U: Result<Vec<AllocatedRelaxedR1CSInstance<G>>, SynthesisError> = U
      .iter()
      .enumerate()
      .map(|(i, U)| {
        let i_alloc = alloc_const(
          cs.namespace(|| format!("U_i i{:?} allocated", i)),
          scalar_as_base::<G>(G::Scalar::from(i as u64)),
        )?;

        let equal_bit = Boolean::from(alloc_num_equals(
          cs.namespace(|| format!("check U {:?} equal bit", i)),
          &i_alloc,
          last_augmented_circuit_index,
        )?);

        conditionally_select_alloc_relaxed_r1cs(
          cs.namespace(|| format!("select on index namespace {:?}", i)),
          U,
          &empty_U,
          &equal_bit,
        )
      })
      .collect();
```

对于第2个电路，核心思路是构造 $$\sum_{i=1}^{10} (rom\_{value[i]}) = rom[pc_i]$$ 来实现其约束, 
其中
$` rom\_value[i]= \begin{aligned} rom[i],\quad i=pc_i \\ 0, \quad i\ne pc[i] \end{aligned} `$

具体代码如下:
```
fn constraint_augmented_circuit_index<F: PrimeField, CS: ConstraintSystem<F>>(
    mut cs: CS,
    pc_counter: &AllocatedNum<F>,
    rom: &[AllocatedNum<F>],
    circuit_index: &AllocatedNum<F>,
  ) -> Result<(), SynthesisError> {

    // select target when index match or empty
    let zero = alloc_zero(cs.namespace(|| "zero"))?;
    let rom_values = rom
      .iter()
      .enumerate()
      .map(|(i, rom_value)| {
        let index_alloc = alloc_const(
          cs.namespace(|| format!("rom_values {} index ", i)),
          F::from(i as u64),
        )?;

        let equal_bit = Boolean::from(alloc_num_equals(
          cs.namespace(|| format!("rom_values {} equal bit", i)),
          &index_alloc,
          pc_counter,
        )?);

        conditionally_select(
          cs.namespace(|| format!("rom_values {} conditionally_select ", i)),
          rom_value,
          &zero,
          &equal_bit,
        )
      })

      .collect::<Result<Vec<AllocatedNum<F>>, SynthesisError>>()?;

    let sum_lc = rom_values
      .iter()
      .fold(LinearCombination::<F>::zero(), |acc_lc, row_value| {
        acc_lc + row_value.get_variable()
      });

    println!("self.circuit index ==============> : {:?}", circuit_index.get_value());
    cs.enforce(
      || "sum_lc == circuit_index",
      |lc| lc + circuit_index.get_variable() - &sum_lc,
      |lc| lc + CS::one(),
      |lc| lc,

    );
    Ok(())

  }
```
## 致谢
非常感谢 SECBIT Labs 的 @郭宇老师对SuperNova研究方向的指导。
## 参考文献
- [Supernova论文](https://eprint.iacr.org/2022/1758)
- [ROM-based Supernova code](https://github.com/microsoft/Nova/pull/204)
- [Nova系列资料](https://github.com/dajuguan/awesome-nova-based-Recursive-Zero-Knowledge-Arguments-knowlege)
