[TOC]
### Overview

**Wordle**：是一个猜词游戏，玩家试图猜测一个 5 个字母的单词。每当玩家猜一个单词时，游戏会告诉玩家哪些字母是正确的（用绿色表示），哪些字母在单词中但位置不对（用黄色表示），以及哪些字母不在单词中（用灰色表示）

#### Motivation：

在某些时候，作者与一些朋友交流他们解出的 Wordle 网格：

![](imgs/zordle/Pasted%20image%2020230919103506.png)

然而，这些表情符表格有一个致命缺陷：玩家可以在游戏结束后编辑他们的网格，让自己看起来比原来聪明得多。我总是怀疑我的朋友们是否真的得到了他们声称的分数！快使用 zk-snark！[^1]

在 Zordle 中，在解决了当天的 Wordle 问题后，用户还会为其表格和 Guess word 生成一个 ZK Proof，证明他们知道与他们共享的一组表情符号框完全对应的一组单词！（In Zordle, after solving the day's Wordle, a user additionally generates a ZK proof attesting that they know the set of words that perfectly correspond to a set of emoji boxes that they're sharing!）

#### BUILD & user flow

- Generate Proof takes about 1 min
- Verify Proof takes about 20s
- Then user can check the proof on chain (IPFS)
- And anyone can verify it

![](imgs/zordle/2023-09-18-151216.png)

```bash
cargo test -- --nocapture test_wordle_1
# Draw
cargo test --release --all-features print_wordle
```

**Inspect ZK Proof:**

 - URL(onchain): https://ipfs.io/ipfs/QmWuSo5ivAXm8M7Mi7hPW5WHFXZ55Vjt651Cw6reL1VM9w
 - When Access the URL, which is a JSON file stored on IPFS : 
```JSON
{
  "solutionIndex":625,
  "proof":[
    109,177,255,176,116,185,157,128,237,146,45,233, ... ,
    247,208,138,100,48,148,37,223,95,80,14,64,239,78, ... , 
    // The proof is very long ,...,
    105,46,209,248,49,117,197,164,130,72,157,40,33,243,21,39,..,
    ],
  "diffs":[[[0,1,0,0,0],[0,1,1,1,0]],[[0,1,0,0,0],[1,1,0,0,0]],[[1,1,1,1,0],[1,1,1,1,0]],[[1,1,1,1,1],[1,1,1,1,1]],[[1,1,1,1,1],[1,1,1,1,1]],[[1,1,1,1,1],[1,1,1,1,1]]]}
```

**Copy URL to clipboard ↗️ :**
```bash
https://zordle.xyz/verify/QmWuSo5ivAXm8M7Mi7hPW5WHFXZ55Vjt651Cw6reL1VM9w
# 👆🏻 with this url, anyone can validate the ZKP proof to ensure that the individual possesses the correct solution, without actually knowing the answer to the Wordle.
```

#### Circuit inputs

**Public inputs**
1. The solution word
2. The grid of boxes of 6 words x 5 slots (one for each letter): each cell in the grid is either green, yellow or grey
	- $\textcolor{green}{green}$ : the letter is in the same relative position as the letter in Solution
	- $\textcolor{orange}{yellow}$ : the letter is in Solution but the wrong relative position
	- $\textcolor{grey}{grey}$ : wrong letter, not in the solution.

like: 
```
1. solution word: 
    "fever"

2. grid of boxes of 6 words x 5 slots
    🟥🟥🟨🟥🟩
	🟥🟥🟩🟨🟩
	🟩🟩🟩🟩🟩
	🟩🟩🟩🟩🟩
	🟩🟩🟩🟩🟩
	🟩🟩🟩🟩🟩
```

**Private inputs**
- 6 words of 5 letters each  (6 个单词，每个单词 5 个字母)

我们注意到：Wordle 的 inputs 结构使得每个 guess(猜测) 都完全独立于 others - 如果一个猜测本身有效，那么在游戏中也总是有效，反之亦然。这表明电路的一种清晰结构是：**make an individual region for each guess.** 

对于这种每个 guess 一个 region 的构建中，让我们考虑每个 guess 需要哪些检查：
> 考虑 该 guess 的 grid  🟥🟥🟩🟨🟩   和 word: "lover"
1. The guess 必须是一个 5 个字母的英语单词 （LOOKUP）
2. 如果格子上的位置是绿色 🟩，则 guess word 相应位置的字母必须与 solution 的字母匹配
4. 如果格子是黄色 🟨，类似的检查也会进行
3. 如果格子不是绿色、黄色，猜测相应位置的字母不能与解答的字母匹配

#### lookup table Versus R1CS 

通常，在 R1CS 电路中，对于存在性证明（比如 Nullifier 的 commitment），需要使用 Merkle Proof 来检查 guess word 是否为字典真实存在的单词：创建一个所有单词（12000+）的 Merkle 树，然后 witness the `Merkle path` of your guess in the tree。

然而，在 PLONK/Halo 2 中，可以使用查找表！虽然以这种方式使用查找表不是特别高效（因为您的电路现在将具有 12000+ 行），but it is a cool way ..

### build demo

Workflow:
1. generate params files like `params.bin` (like verification_key / proving_key ...)
2. use your wordle answer to generate proof.
3. verify the proof you generated.

#### 1. create a `proof.bin`

Firstly, we need to manually create a `proof.bin` file ourselves, otherwise the `$ cargo run` command will report an error.

```bash
cd circuits
touch proof.bin
```

#### 2. generate public params

```bash
$ cargo test -- --nocapture test_wordle_1
$ cargo run 
write  #  take ~3 min to generate `params.bin` and `diffs_json.bin`,

# Welcome to zk wordle!
# Enter play to play the game, verify to check a proof, or write to generate a new # # params file
# write
```

#### 3. gen proof (if guessed)

```bash
$ cargo run 
play  # correct input : fluff
Welcome to zk wordle!
Enter play to play the game, verify to check a proof, or write to generate a new params file
play
Enter a word:
proof
🟥🟥🟥🟥🟩
Enter a word:
leave
🟨🟥🟥🟥🟥

Enter a word:
belif
🟥🟥🟨🟥🟩
Enter a word:
Foulf
🟥🟥🟩🟨🟩

// ... many rounds...
Enter a word:
fluff
🟩🟩🟩🟩🟩
You win! Generating ZK proof...
Successfully generated witness
Successfully generated proving key
Successfully wrote proof to proof.bin
Verifying proof for final word fluff
Share Sheet:
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
Proof OK!

# or You lose! and exit.
```

#### verify proof

Then we acn verify
```bash
# verification takes about 15s, 1GB Memory
Welcome to zk wordle!
Enter play to play the game, verify to check a proof, or write to generate a new params file
verify
Verifying proof for final word fluff
Share Sheet:
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
Proof OK!
```


在我找到了正确答案并生成 proof 的过程中，如果我强制退出 generate_proof 程序，在验证时：
```
verify
Verifying proof for final word fluff
Share Sheet:
🟥🟩🟩🟥🟥
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
🟩🟩🟩🟩🟩
Proof not OK!
```
会出现奇怪的 sheet，且 verification 不通过，原因不明

#### wasm

【EDITING】

### files directory & Code explanation

`$ tree` show the code structure : 

```rust
├── lib.rs
├── main.rs // play(gen prove-prove_play) verify  write_params
├── wasm.rs 
├── wordle
│   ├── wordle
│   │   ├── dict.json  // 12972 个英文单词， 如 “white”
│   │   ├── dict.rs    // [738547, 742032, ..., 760311, 760617, 760805 ..
│   │   ├── is_zero.rs // IsZeroChip
│   │   ├── table.rs   // Lookup table, 将 12972 个 5 字母 word 放入查找表
│   │   └── utils.rs   // word_to_chars, compute_diff..
│   └── wordle.rs
└── wordle.rs  // pub mod wordle;
```

#### Lookup table - table.rs

lol perhaps best thought LOOKUP table of as a giant fixed set(constant set) instead of a circuit table column.


作用： 将 12972 个 `5-letter` words 加载到 LOOKUP 查找表里。
> 这些 words 的形式类似： `vec![738547, 742032, ...  , 760311, 760617, 760805,... `

```rust
#[derive(Serialize, Deserialize)]
struct Dict {
  words: Vec<String>,
}
impl<F: PrimeField> DictTableConfig<F> {
  pub(super) fn load(&self, layouter: &mut impl Layouter<F>) -> Result<(), Error> {
    // 12972 个 5-letter words, like [783431, 2149761, 11454874]..
    let mut words = get_dict(); 
    words.push(0);

    layouter.assign_table(
      || "load dictionary-check table",
      |mut table| {
        let mut offset = 0;
        for word in words.iter() {
          table.assign_cell(
            || "num_bits",
            self.value,
            offset,
            || Value::known(F::from(word.clone() as u64)),
          )?;
          offset += 1; 
        }
```

#### dict

`wordle/wordle/dict.rs` : 

```rust
pub fn get_dict() -> Vec<u32> {
    vec![738547, 742032, 747019, 747397, 756988, 
    756996, 756998, 757006, 757094, 757220, 757293, 
    757310, 757456, 757459, 757462, 757485, 757626, 
    757789, 757890, 757905, 757911, 758196, 758732,
    760306, 760311, 760617, 760805, 760863, 763240,
    763749, 763792, 766300, 766314, 766315, 766316,
    766609, 767239, ........
```

`wordle/wordle/dict.json` : ``

```rust
{"words": [
    "aahed",
    "aalii",
    "aargh",
    "aarti",
    // ....
    // ....
    // ....
}
```

#### wordle.rs

```rust
#[derive(Debug, Clone)]
/// A range-constrained value in the circuit produced by the RangeCheckConfig.
struct RangeConstrained<F: PrimeField>(AssignedCell<Assigned<F>, F>);
```

##### Constraints (custom gate)

![](imgs/zordle/Pasted%20image%2020230919161116.png)

如上图，在 Custom gate 编写电路约束时，会为**每一个 Guess word** 在 region 里分配如上图这样一个布局。
- 本轮 Guess word 是  $\textcolor{green}{f}\textcolor{orange}{u}nky$ 
- Solution(final_word) 是  $fluff$
- 电路会去计算 & 约束各种配置 ...

##### assign to region

```rust
/// ......
// make an individual region for each guess.
for i in 0..WORD_LEN {
	// guess word, provided by the user. place on the row-0
	region.assign_advice(|| "input word characters", self.chars[i], 0, || chars[i])?;
	// solution word, provided by the user. place on the row-1
	region.assign_advice_from_instance(|| "final word characters",
		self.final_word_chars_instance, i, self.chars[i], 1)?;
	// diff_green: if guess[i] matches solution[i], then diff_green is 0, Otherwise is the distance between 2 chars
	region.assign_advice(|| "diff_green", self.chars[i], 2, || diffs_green[i])?;
	// diff_yellow: if guess[i] ∈ solution, then diff_yellow is 0, Otherwise is the distance multiplication
	region.assign_advice(|| "diff_yellow", self.chars[i], 3, || diffs_yellow[i])?;
/// ......
```


#### Circuit 

```rust
pub struct WordleCircuit<F: PrimeField> {
    pub poly_words: [Value<Assigned<F>>; WORD_COUNT],
    pub word_chars: [[Value<Assigned<F>>; WORD_LEN]; WORD_COUNT],
    pub word_diffs_green: [[Value<F>; WORD_LEN]; WORD_COUNT],
    pub word_diffs_yellow: [[Value<F>; WORD_LEN]; WORD_COUNT],
}

impl<F: PrimeField> Circuit<F> for WordleCircuit<F>{
    fn without_witnesses(&self) -> Self { .. }
    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config { .. }
    fn synthesize(
        &self,
        config: Self::Config,
        mut layouter: impl Layouter<F>,
    ) -> Result<(), Error> {
        config.table.load(&mut layouter)?;

        for idx in 0..WORD_COUNT {
            // println!("idx {:?} diffs_green: {:?}", idx, self.word_diffs_green[idx]);
            config.assign_word(
                layouter.namespace(|| format!("word {}", idx)),
                self.poly_words[idx],
                self.word_chars[idx],
                self.word_diffs_green[idx],
                self.word_diffs_yellow[idx],
                idx,
            )?;
        }
        Ok(())
    }
```


### illustration

Circuit :



### QA： 

-----

Why Rotation(-2)？就具体在这个 wordle 电路里面，你试着理解和推理下，为什么要使用 Rotation(-2)

在给出的 `WordCheckConfig` 电路配置中，`Rotation(-2)` 被用于几个地方。这些用途都与字符差异和颜色判断有关。让我们深入挖掘并理解它的具体用途。

1. **字符差异检查**：  
当比较Wordle中的字符以确定绿色和黄色的匹配时，我们通常会查看所猜测的单词与实际的目标单词之间的字符差异。`Rotation(-2)` 似乎被用于访问电路中早期的某个位置，该位置可能包含前一个单词或猜测的单词。

`let char = meta.query_advice(chars[i], Rotation(-2));`

2. **颜色为零检查**：  
代码中的 `Rotation(-2)` 也被用于与颜色检查相关的部分。在Wordle中，字符可能是绿色、黄色或灰色。灰色表示字符不在目标单词中。绿色表示字符位置正确，而黄色表示字符存在但位置不对。`Rotation(-2)` 可能被用来查询前一个猜测中的字符颜色或与之相关的其他信息。

`let diff_color_is_zero = meta.query_advice(chars[i], Rotation(-2));`


通过这两点，我们可以推测为什么使用 `Rotation(-2)`：

电路的每一行可能代表一个字符位置的不同状态或检查。在检查猜测与实际单词之间的匹配时，电路需要对照早先的行（可能是前一个猜测或前两个状态）。这就是为什么需要查询 `Rotation(-2)`：它代表与当前字符或状态相对的早先的状态或检查。

具体地，这里的 `Rotation(-2)` 可能表示查询与当前字符位置前两个位置或状态相关的数据，这有助于确定字符的正确性、颜色或其他属性。




### References


[^1]: Ignore the minor technical detail that they can always just cheat by looking up the day's word elsewhere. 😅
