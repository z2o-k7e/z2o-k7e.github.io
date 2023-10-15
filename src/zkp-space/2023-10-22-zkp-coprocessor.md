# 聊聊 ZKP Coprocessor

> Author: [Harry](https://twitter.com/harryx1x1)
>
> Label: ZKP, ZKP Coprocessor, ZKP Space
>
> Date: 2023-10-15

回放链接: [https://twitter.com/i/spaces/1rmxPMaezNnKN?s=20](https://twitter.com/i/spaces/1rmxPMaezNnKN?s=20)

主聊人:
- [Harry](https://twitter.com/harryx1x1)
- [0xhhh](https://twitter.com/hhh69251498)
- [Cecilia](https://twitter.com/ceciliaz030)
- [董沫博士(Dong)](https://twitter.com/no89thkey)

## Harry：什么是 ZKP coprocessor？
Dong
 - 是区块链的 coprocessor
- 能让智能合约有 dune analytics 的能力
- 现在合约面临的问题是合约不能看到：
		- 历史数据
		- 没有 public viewable 的数据
		- 其它链的数据
- example：为什么要有 ZKP coprocessor
	- dex 需要有 trading volume based filter 的事情
		- 没有基于交易量的一些功能
			- 比如打折等
		- 而这些功能对于 dex 很重要
			- 手续费打折可以吸引更多的流动性
			- 更多的流动性可以产生更有效的市场价格
	- 为什么 dex 没有呢
		- 因为难
		- 但 cex 实际是很简单的
		- dex 两种方式做
			- 记住每个 trade action
				- 在 uniswap 试验过，每个 trading fee 都贵三倍
			- on the fly
				- 每个 trade 加 counter，做 on the fly 的功能
				- 问题：
					- 给运营的功能加到核心业务里，这是一种 overhead，增加 trader 的成本
		- 所以没有任何的 dex 做这件事
	- 其实理论上，用区块链本身也能做
		- 通过将 block header 存储到一个 Merkle Tree 上面
		- 用到的时候再去查找
	- 但实际上，这个方案不可行，查询一天前的一个交易就要花费 8 百万的 gas fee
- 只要基于用户的过去行为的逻辑，在区块链上都是不可能
- 这就是为什么要引入 ZKP coprocessor
- 计算的事情是在链下做的，不是在链上
- 逻辑写到一个电路里面
	- 比如证明用户在 5 天交易了超过 1000 笔
- public input 可以是一些 anchor point，比如某个时间段，这样可以给出这个时间段的证明，节省一些证明的时间
- 这样可以给出证明
- 为什么叫 coprocessor，这个和计算机发展有点类似，gpu handle cpu 不擅长的事情
- ZKP coprocessor 就是为了解决区块链不擅长的计算，比如 data intensive 的计算:
		- 大量的数据
		- 复杂 stateless 的计算
			- 计算过程的中间值不需要存储到链上
			- 只需要把最终的计算结果放到链上

## 0xhhh：ZKP coprocessor 和 rollup 的关系是什么？
Dong
 - 虽然广义上都是做链下计算，然后链上有一个 anchor
- 但它们要解决的问题不一样
- 主要的区别在于 state 有多重
- rollup 是有 EVM equivalent 的需求的
- 在 rollup 里面也是同样不适合做 data intensive 的计算
- 而 ZKP coprocessor 没有持久的数据存储，计算完成将结果返回给调用方就完成任务了，不需要存储下来
- 有些系统和 ZKP coprocessor 很像，比如 zkvm 和 zkwasm
- 在 coprocessor 领域，单纯的用一个 vm 是不行的，因为需要的 overhead 是非常大的
	- RiskZero 已经很快了
- 未来的 coprocessor
	- 对链上数据的计算可能需要定制化的电路和轻量级的 vm 结合的方式。
		- 定制化的电路去做什么呢？去做一些这种非常核心的，经常会要在 coprocessor 当中用到的，比如说 hash，Merkle Tree 的 inclusion，signature verification
	- 然后可以把 vm 当一个 aggregation layer
	- vm 上面给开发者一个友好的平台，提供一些接口，比如可以拿历史的 trade 数据做简单的加减运算
- 有一个需求也在 rollup 上面也部署一个 ZKP coprocessor

Cecilia：技术上补充一下
	- 拿 Axiom 来说
		- 只去执行 view 函数的证明
		- 不去做 state transition 的证明（不做 MPT 的状态转换）
		- 所以这样的 ZKP coprocessor 在执行逻辑和安全假设上，可以看做为 rollup 的子集
	- 而如果把 view 函数的证明放到一个广义的 VM 上面，比如 RiscZero 的 zkvm
		- 从执行能力（execution power）上，可以看做是 EVM 的 super set
	- RiscZero 是针对 general purpose 运算，不只是针对区块链

## ZKP processor 的三种 proof
Dong
- ZKP processor 主要做三种 proof，以及这三种的排列组合
	- merkle state proof
		- 比如过去某个时间点 NFT 的 holder 是谁
		- 需要去 storage 去找这个过去的 value
	- transaction proof
		- transaction 也有一个 tree，也可以做证明
		- 不管 transaction 是否成功链上都有记录
	- transaction receipt proof
		- 成功后有 receipt，可以通过 event 发出来
		- 从这里可以看到交易了多少 token，这些可以用来计算 trading volume
- Axiom 复用了 zkevm 中常用的电路
	- 向 zkevm 借鉴了一下电路的代码，也贡献了一些代码，比如 merkle tree inclusion 的代码
	- 从 zkevm 拆出来了一些电路，费了很大力气
		- 因为要做列的对齐
	- 也做了很多优化
		- lookup 优化
- 几个做 ZKP coprocessor 的团队
	- Brevis
	- Axiom
	- Herodotus
	- Lagrange
- 各个团队做的方式都不一样，比如用的 framework 和 scaling solution 方式都不一样
- 大家在解决的技术问题
	- 基于一万个 transaction 做计算
	- 并以很低的成本去证明这些计算

## Prover 性能优化
Dong
- 做性能优化，三条路
	- 算法层
		- 用 halo2 怎么办
			- 可以用 lookup 优化
			- Axiom 使用了 [Merkle Mountain Range (MMR)](https://docs.grin.mw/wiki/chain-state/merkle-mountain-range/)，不断的去 commit root hash
			- Brevis 使用 Mimic hash
	- effective aggregation
		- 五花八门
		- 简单的多加两层 recursion，再加并行化
		- 有一些 aggregation 是可以用的
			- chain of curves：用两个 curve 导一下，试下来不比 folding 差
		- 能把一万个 transaction 压缩到分钟级别的证明
	- 还可以用 folding，比较适合 coprocessor，但 folding 还差的比较远
  	- 差 parallelization
  	- 不能上链。现在接的 IPA，但需要改成 groth16，但是个大工程
### 使用 Folding 提升性能
0xhhh
- RiscZero 说用 folding 需要的带宽比较多
- 用 prover 生成 proof，需要有个机器去管理 state 并分发给其它的 prover 生成 proof
Dong
- folding 处在三个和尚没水吃的阶段
	- 大家都觉得少点东西，但工作量大，谁来加呢，因为开源后，大家都会用
	- 所以 PSE 这样的比较公益的组织在做
	- 但这个事情很有意义，有人组织大家一起做也是不错的
Cecilia
- 大家对 folding 太着迷了
- 在 recursion 和 folding 之间，真正在 engineer 上面我们在 focus on 的一个东西叫就是 chunk prover，这是 Taiko 和 PSE 当前的 focus
- 所作的事情就是如何把 execution 分成 chunk，然后就可以并行执行，并证明
- 最后把所有的 chunk 的执行接到一起，生成一个证明
- 这个其实是在目前工程中比较实际的做法，而不是用 folding
Dong
	- 赞同通过 continuation 来实现并行
	- folding 目前短期来看投入产出比比较低
Cecilia
- 其实我觉得只要在学术上，在理论上有一个基本的 support 能够把 combination 本身拆开，那在工程上面拆合就有很多 engineer 可以干的事情，engineer 可以把 prover 性能优化提升 100 倍
- 我对 folding 的态度暂时是观望
Dong
- 还有一个提升性能方式是用硬件加速
	- halo2 没有好的硬件加速
- Ingonyama 的 icicle 挺好用的，有合作
	- 思路比较简单
		- 把常用的算子放到 GPU 中
	- 效果有 20 倍的提升
	- Cecilia 也在实现 low level 的 gpu 加速 的 api

## Franci：ZKP coprocessor 和 the graph 和类似？
Dong
	- 大体上可以这样理解
	- 但 the graph 安全性没有 ZKP coprocessor 高，因为你要信任跑节点的一方

## 0xhhh：ZKP coprocessor 有哪些 use case？
Dong
- AMM DEX
	- 问题
		- 从 LP staking 看不出来 LP 对 liquidity pool 的贡献
		- 因为只看 staking 的数量不准确
	- 好的 liquidity incentive design
		- 回头看这一个月的情况去给奖励
		- 对 pool 贡献多的才给激励
- 游戏
	- 传统的游戏除了内容之外，有两个支柱
		- 买量（获取用户）
			- 都是一锤子买卖，从广告商（youtube）买一个人，cost per install，安装一个就付钱就完了，不能再继续结合后续用户参与游戏的情况来做更多的收益分成和合作
			- 通过 coprocessor 可以改变这个单次博弈的状态
		- 运营
			- 弹窗问用户是否买 token 或者皮肤
			- 游戏体验是根据用户的历史交互决定的
	- 它们是数据驱动的
	-  ZKP coprocessor 是可以解决这个问题的
	- 可以提供给游戏用户定制化的体验
- Social & Identity
	- 根据用户在一个链上的 social 历史数据作为另外一个链上用户的身份证明
- 广泛的说，如果需要用到非同步计算，并且计算量比较大，都可以放到 coprocessor 里面做

## Dong：如何让开发者基于 ZKP coprocessor 开发
- 让所有开发者都做电路的开发不太现实
- 如何解决这个问题，大家也有自己的方式
	- Brevis 采用的是 specialized circuit + lightweight VM 的模式。coprocessor 的基底是很多的 specialized 的 circuit，比如 merkle tree 等
	- 还有的是先做一个 use case，比如做 DEX trading volume 数据相关的 API，把这个场景涉及到的功能封装成 API 一下给开发者用。好处是开发者用着比较方便，缺点是只适用于这一个场景，其它的场景需要单独做

## ZKP coprocessor 和游戏
Cecilia
- 因为游戏的执行对 consensus 安全性的要求是 relax 的，其实不需要像 rollup 这么频繁的提交状态，但是对 performance 的要求很高。对 performance 的这个执行环境的需求肯定是要比 ZKEVM 要 profound 很多，可能需要用 rust 代码去写，可能甚至要在 GPU 上面去跑。我觉得就是一个非常有意思的，可以 apply to ZKP coprocessor 的场景

Dong
还有些方式解决 scalability 的问题，比如用 optimistic 的方式，先相信节点的计算，如果有问题可以后面进行挑战，这种模式适用于有时间去做挑战的场景。

Cecilia
其实包括 rollup 也是这样的，你现在 Taiko 所选择的这个实现方式，我们现在叫 contestable rollup，就是随机选块去生成 ZK 证明，而不是每一个块都去 ZK 它，然后有的块用 SGX 去证明，在整个这个 space 里面就是你要去相信或者不相信一个 remote execution the result，这个完全是取决于你用什么样的机制去 secure 它，POW 还是 POS 还是 proof of ZK 这个东西完全是取决于实现。然后我觉得对于 Gaming 来说，肯定是要牺牲更多的安全性和 trust 去换他的这个optimization。

0xhhh
- 感觉现在整个以太坊就像一个都在朝着 coprocessor 去扩展，比如说就以太坊一直在说 PBS，实际上本质上我觉得也算是一种 coprocessor，它算是 proposal 不需要去存储整个区块链状态了，只有 builder 需要去存储整个区块链的状态，所以感觉我现在就觉得好像整个以太坊都在 coprocessor
Dong
- 好像是最近的思潮
- 都是问一个问题
	- 到底什么东西要放在什么地方
	- 什么计算放到以太坊上比较好
	- 通过功能的拆分，来做更好的状态和计算上的 scalability
- 我觉得可能是这个 rollup 的大的这个蓝图已经确立了之后，大家在思考如何再进一步，在已有的这个蓝图里面，怎么去从不管是从系统层面还是从架构层面更好的去优化 performance 和 scalability。我觉得可能是这个思潮驱动的。
0xhhh
- 我觉得确实应该说从 rollup 或者从一开始状态通道以来，其实整个区块链的发展方向，因为很慢的结算网络意味着支持比较大的计算都只能靠 coprocessor 化，所以感觉这是持续了很多年的思潮了，只不过今天可能真正给它定义成 coprocessor，大家对这个方向有一个比较明确的共识，都在推动整个以太坊的整个 layer 1 协议往 coprocessor 的方向去演变

## ZKP bridge
Dong
- ZKP bridge
	- 本质是 bridge 一个 block header，中间的 VM 没有什么必要，只需要一个 aggregation 功能，把支持的各个链的 block header aggregate 就好了
	- 开发不 scalable，需要针对每个链做一些特殊的定制方案
	- 开发的过程中发现了 coprocessor 的需求
Cecilia 补充关于 ZKP bridge
- 假设要 bridge 一个以太坊的 block header
- public input：下一个区块的 block hash
- advice：当前区块的 BLS signature
- aggregation：aggregate 所有的 signature
- polyhedra 的 zkIBC 做出来了，是因为做了一套可以很快的 aggregate 以太坊 BLS signature 的电路。和 zkevm 没有关系
- 跨链验证的是共识，不是执行

## Ingonyama
Dong
- 性能和体验还是不错的
- 实现了很多 curve
Cecilia
- 用 GPU 封装算子这个事情很简单
- 很多从矿场出来的团队都已经积累的相关的技术
- 但为什么大家都选择了 Ingonyama？
Dong
- 因为 Ingonyama 是开源的，代码采用了 MIT 协议

## 参考
提到的一些zk项目和团队list：
- [https://brevis.network/](https://brevis.network/)
- [https://taiko.xyz/](https://taiko.xyz/)
- [https://www.axiom.xyz/](https://www.axiom.xyz/)
- [https://www.risczero.com/](https://www.risczero.com/)
- [https://delphinuslab.com/zk-wasm/](https://delphinuslab.com/zk-wasm/)
- [https://www.moduluslabs.xyz/](https://www.moduluslabs.xyz/)
- [https://lurk-lang.org/](https://lurk-lang.org/)
- [https://www.ingonyama.com/](https://www.ingonyama.com/)
- [https://manta.network/](https://manta.network/)
- [https://zkbridge.com/](https://zkbridge.com/)

感谢 Kurt 对部分内容的贡献