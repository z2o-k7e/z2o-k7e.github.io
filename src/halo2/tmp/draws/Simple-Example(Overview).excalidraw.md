---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
FieldChip{
  config,
  _marker
} ^IUmcDwKM

FieldConfig {
  advice, instance,
  constant, s_mul, 
} ^1omZvCkn

impl FieldConfig {
  fn configure()
} ^iYVKAYha

meta.enable_equality
meta.create_gate ^eYB9KP9W

添加约束, 定义 gate ^9wqgTBe5

trait NumericInstructions: Chip ^KjtrBGEa

trait ^VLTgV4fC

type Num; ^qSQKTKIT

fn load_private ^Nwuyu6hm

fn load_constant ^2PYaZncJ

fn mul // Returns c = a*b  ^C4zzPUdg

fn expose_public ^ew0dzhpR

Number {
  cell: Cell,
  value: Option<F>
} ^p4FEsScv

impl Chip for FieldChip {
  type config = FieldConfig;
  fn loaded
} ^BssSKIDJ

芯片存储它们自己的配置 ^QLRAGNiq

impl NumericInstructions for FieldChip { 
  type Num = Number<F>;
  fn load_private() { ... }
  fn load_constant() { ... }
  fn expose_public() { ...}

  fn mul()
} ^zYwpRRXv

layouter.assign_region ^fbdGsxS4

FieldChip ^NA9bi0GI

let rhs = region.assign_advice("rhs") ^jwI34B1N

region.constrain_equal(a.cell, lhs) ^1FXiQWwe

out = Some(Number { cell, value }); ^dsBy4KS2

let lhs = region.assign_advice("rhs") ^GhO4mrnc

region.constrain_equal(b.cell, rhs) ^6SYPEEq8

let value = a.value.and_then(|a| b.value.map(|b| a * b)); ^IVTDtjnM

let cell = region.assign_advice( || "lhs * rhs",
    config.advice[0], 1, value ), ^8eWh79tf

MyCircuit {
    constant: F,
    a: Option<F>,
    b: Option<F>,
} ^Jk05BIfN

impl<F: FieldExt> Circuit<F> for MyCircuit {
   type Config = FieldConfig;
   type FloorPlanner = SimpleFloorPlanner;


   fn configure()




   fn synthesize ^cNvLNTho

let advice = [meta.advice_column(), meta.advice_column()];
let instance = meta.instance_column();
let constant = meta.fixed_column();
FieldChip::configure(meta, advice, instance, constant) ^QgpSFcaI

 // load Private/Constant values :
 let a = field_chip.load_private(layouter.namespace(|| "load a"), self.a)?; 
 let b = field_chip.load_private(layouter.namespace(|| "load b"), self.b)?;
 let ab = field_chip.mul(layouter.namespace(|| "a * b"), a, b)?;
 let absq = field_chip.mul(layouter.namespace(|| "ab * ab"), ab.clone(), ab)?;
 // 将结果作为电路的公开输入进行公开
 field_chip.expose_public(layouter.namespace(|| "expose c"), c, 0) ^NlzZ9f97

 // load Private/Constant values : ^4v6OryrH

 // 将结果作为电路的公开输入进行公开 ^51MS8lu4

Chip ^7hhy6ZXu

NumericInstructions ^WZrUB9ss

trait NumericInstructions: Chip ^WqjNCZek

trait ^VqfswGJL

type Num; ^S7BOTS0N

fn load_private ^DuSeLb2Z

fn load_constant ^MyCjQPRl

fn mul // Returns c = a*b  ^vB8pIC14

fn expose_public ^86lLPp3D

NumericInstructions ^PCtEg9Yq

指令集 ^2FtvK7hx

FieldChip{
  config,
  _marker
} ^f4LpiJl2

impl Chip for FieldChip {
  type config = FieldConfig
  fn loaded
} ^w05hUMu5

芯片(实现在有限域上) ^q8WArw7a

Chip ^7zCLaav0

FieldConfig ^XVr4ox7S

配置
芯片 ^vwOvzJz0

FieldConfig {
  advice, instance,
  constant, s_mul, 
} ^yqHSWGOl

impl FieldConfig {
  fn configure()
} ^E9ycu1jU

meta.enable_equality
meta.create_gate ^57wXf6x9

添加约束, 定义 gate ^tlxZ3QKN

配置芯片：
1. 挑选所需元件
2. 根据功能逻辑配置其连接排布方式 ^BH2hPoz7

(①选取所需元件) ^n0DxIMUh

(② 连接 排布) ^MHyuCHw1

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.8.17",
	"elements": [
		{
			"type": "rectangle",
			"version": 527,
			"versionNonce": 33842228,
			"isDeleted": false,
			"id": "B-MGzxPMR4j4etIPVU_Zm",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -562.5940291017712,
			"y": -485.9232498828968,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 177.44967413552595,
			"height": 107.17786697419095,
			"seed": 1162515373,
			"groupIds": [
				"sHJwV4fKevT1NfmPcUVND"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 477,
			"versionNonce": 1510615308,
			"isDeleted": false,
			"id": "hfANWtZPezI38QQpLAxtY",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -562.3531273638812,
			"y": -485.68568391646056,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 2031847843,
			"groupIds": [
				"sHJwV4fKevT1NfmPcUVND"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 216,
			"versionNonce": 1104264628,
			"isDeleted": false,
			"id": "IUmcDwKM",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -551.326540173878,
			"y": -480.0820941178066,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 91.162109375,
			"height": 96,
			"seed": 1601541645,
			"groupIds": [
				"sHJwV4fKevT1NfmPcUVND"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "FieldChip{\n  config,\n  _marker\n}",
			"rawText": "FieldChip{\n  config,\n  _marker\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FieldChip{\n  config,\n  _marker\n}"
		},
		{
			"type": "rectangle",
			"version": 705,
			"versionNonce": 1266231180,
			"isDeleted": false,
			"id": "gIJnUVi8fln1JcKE_syFN",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -563.4073600162681,
			"y": -352.9300779685351,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 177.44967413552595,
			"height": 107.17786697419095,
			"seed": 752901443,
			"groupIds": [
				"Lcyq52bR6SP6Kq1JqQABf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 655,
			"versionNonce": 565700404,
			"isDeleted": false,
			"id": "fEc2T6u-ViXhtnpzJ_js1",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -563.1664582783781,
			"y": -352.69251200209885,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 1439779949,
			"groupIds": [
				"Lcyq52bR6SP6Kq1JqQABf"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 426,
			"versionNonce": 1669617164,
			"isDeleted": false,
			"id": "1omZvCkn",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -552.2647859406766,
			"y": -347.2138370557467,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 162.294921875,
			"height": 96,
			"seed": 604617955,
			"groupIds": [
				"Lcyq52bR6SP6Kq1JqQABf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "FieldConfig {\n  advice, instance,\n  constant, s_mul, \n}",
			"rawText": "FieldConfig {\n  advice, instance,\n  constant, s_mul, \n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FieldConfig {\n  advice, instance,\n  constant, s_mul, \n}"
		},
		{
			"type": "text",
			"version": 5735,
			"versionNonce": 1176297652,
			"isDeleted": false,
			"id": "iYVKAYha",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -364.0148874272961,
			"y": -324.34224349152623,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 124.5,
			"height": 57.599999999999994,
			"seed": 1210529485,
			"groupIds": [
				"M2AEWzCJ5oetHdFrClFYG"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl FieldConfig {\n  fn configure()\n}",
			"rawText": "impl FieldConfig {\n  fn configure()\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl FieldConfig {\n  fn configure()\n}"
		},
		{
			"type": "rectangle",
			"version": 3735,
			"versionNonce": 1596306572,
			"isDeleted": false,
			"id": "Q-EZh1FxJQVCu23jIa-VC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -373.7663729121714,
			"y": -338.0333110261894,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 145.52720563443728,
			"height": 86.47529221462202,
			"seed": 438708355,
			"groupIds": [
				"M2AEWzCJ5oetHdFrClFYG"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 905,
			"versionNonce": 1453763124,
			"isDeleted": false,
			"id": "jUXLSs9sTbMEbqLlcJqzn",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -260.9074512221206,
			"y": -294.40832632180377,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 61.57745033611309,
			"height": 3.6323822146966904,
			"seed": 554751277,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"focus": -0.35917948376190606,
				"gap": 1.0000000000000568,
				"elementId": "eYB9KP9W"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					61.57745033611309,
					3.6323822146966904
				]
			]
		},
		{
			"type": "text",
			"version": 511,
			"versionNonce": 1276592908,
			"isDeleted": false,
			"id": "eYB9KP9W",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -198.33000088600738,
			"y": -313.9382385443017,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 152.109375,
			"height": 38.4,
			"seed": 1425285155,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "meta.enable_equality\nmeta.create_gate",
			"rawText": "meta.enable_equality\nmeta.create_gate",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "meta.enable_equality\nmeta.create_gate"
		},
		{
			"type": "text",
			"version": 310,
			"versionNonce": 372982708,
			"isDeleted": false,
			"id": "9wqgTBe5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -373.2174574053149,
			"y": -362.07124803838417,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 140.4765625,
			"height": 19.2,
			"seed": 1006086029,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "添加约束, 定义 gate",
			"rawText": "添加约束, 定义 gate",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "添加约束, 定义 gate"
		},
		{
			"type": "rectangle",
			"version": 346,
			"versionNonce": 1892876684,
			"isDeleted": false,
			"id": "hw7HzS0Rlvzgq4qhYTbvN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -719.3839446438232,
			"y": -192.00514281073004,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 228.00408614309194,
			"height": 35.62634919819072,
			"seed": 1583489987,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 327,
			"versionNonce": 2063641908,
			"isDeleted": false,
			"id": "KjtrBGEa",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -714.530429018823,
			"y": -183.10910816846024,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 214.2890625,
			"height": 19.2,
			"seed": 1259679213,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "trait NumericInstructions: Chip",
			"rawText": "trait NumericInstructions: Chip",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "trait NumericInstructions: Chip"
		},
		{
			"type": "text",
			"version": 278,
			"versionNonce": 882044940,
			"isDeleted": false,
			"id": "VLTgV4fC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -714.5942543536445,
			"y": -183.6678628432329,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 26.671875,
			"height": 19.2,
			"seed": 2123281251,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "trait",
			"rawText": "trait",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "trait"
		},
		{
			"type": "arrow",
			"version": 1014,
			"versionNonce": 1180493492,
			"isDeleted": false,
			"id": "2-tp6OvlU6CHl31eeZW3S",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -494.6012082915695,
			"y": -186.04438899374566,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 29.41856284071116,
			"height": 14.779215115203556,
			"seed": 2104682573,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"startBinding": {
				"focus": 0.6956847505976528,
				"gap": 5.640158227253551,
				"elementId": "KjtrBGEa"
			},
			"endBinding": {
				"focus": -0.08480003277614989,
				"gap": 30.026454891415426,
				"elementId": "56pd4DrA-JRfeKV5SXlQm"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					29.41856284071116,
					-14.779215115203556
				]
			]
		},
		{
			"type": "text",
			"version": 331,
			"versionNonce": 122752652,
			"isDeleted": false,
			"id": "qSQKTKIT",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -459.93416176160076,
			"y": -217.31673910720258,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 72.9140625,
			"height": 19.2,
			"seed": 1976061699,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "type Num;",
			"rawText": "type Num;",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "type Num;"
		},
		{
			"type": "arrow",
			"version": 1558,
			"versionNonce": 485879860,
			"isDeleted": false,
			"id": "qiHy3Exk37It9tmklfNnh",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -493.3604298289831,
			"y": -185.45665464024717,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 25.18176725233525,
			"height": 1.0231282479757056,
			"seed": 1693079213,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"startBinding": {
				"focus": -1.1882799002836968,
				"gap": 6.880936689839928,
				"elementId": "KjtrBGEa"
			},
			"endBinding": {
				"focus": 1.15275612759286,
				"gap": 7.79805551968775,
				"elementId": "2PYaZncJ"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					25.18176725233525,
					1.0231282479757056
				]
			]
		},
		{
			"type": "text",
			"version": 562,
			"versionNonce": 1237802252,
			"isDeleted": false,
			"id": "Nwuyu6hm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -460.19075904310216,
			"y": -196.75056808044735,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 104.9609375,
			"height": 19.2,
			"seed": 1988982435,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn load_private",
			"rawText": "fn load_private",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn load_private"
		},
		{
			"type": "arrow",
			"version": 719,
			"versionNonce": 514967988,
			"isDeleted": false,
			"id": "udMr4u0_J_q55kPkZttxV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -492.68041213089975,
			"y": -170.66027217834255,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 27.794787099778887,
			"height": 3.4086045337234054,
			"seed": 472185101,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495169,
			"link": null,
			"locked": false,
			"startBinding": {
				"focus": -0.49332514549489975,
				"gap": 7.560954387923289,
				"elementId": "KjtrBGEa"
			},
			"endBinding": {
				"focus": 0.3500453407503545,
				"gap": 8.890591833920439,
				"elementId": "C4zzPUdg"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					27.794787099778887,
					3.4086045337234054
				]
			]
		},
		{
			"type": "text",
			"version": 343,
			"versionNonce": 896523148,
			"isDeleted": false,
			"id": "2PYaZncJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -460.3806070569601,
			"y": -177.51503050936947,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 117.421875,
			"height": 19.2,
			"seed": 1603054147,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn load_constant",
			"rawText": "fn load_constant",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn load_constant"
		},
		{
			"type": "text",
			"version": 315,
			"versionNonce": 2125886260,
			"isDeleted": false,
			"id": "C4zzPUdg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -460.66709755260854,
			"y": -158.3610758106987,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 176.5234375,
			"height": 19.2,
			"seed": 748441453,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn mul // Returns c = a*b ",
			"rawText": "fn mul // Returns c = a*b ",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn mul // Returns c = a*b "
		},
		{
			"type": "arrow",
			"version": 761,
			"versionNonce": 1149536780,
			"isDeleted": false,
			"id": "UuhDFAPPU1XdFSNFMIU_M",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -493.8958836710783,
			"y": -167.0894173706796,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 27.80502439300369,
			"height": 19.782646113073724,
			"seed": 899967459,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"startBinding": {
				"focus": -0.8659571116988269,
				"gap": 6.345482847744734,
				"elementId": "KjtrBGEa"
			},
			"endBinding": {
				"focus": -0.5258611347222364,
				"gap": 8.791987742245283,
				"elementId": "ew0dzhpR"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					27.80502439300369,
					19.782646113073724
				]
			]
		},
		{
			"type": "text",
			"version": 258,
			"versionNonce": 459926708,
			"isDeleted": false,
			"id": "ew0dzhpR",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -461.61367590753275,
			"y": -138.5147835153606,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 120.0859375,
			"height": 19.2,
			"seed": 882497997,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn expose_public",
			"rawText": "fn expose_public",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn expose_public"
		},
		{
			"type": "arrow",
			"version": 751,
			"versionNonce": 436168844,
			"isDeleted": false,
			"id": "AM1v3MlxfT4rZUC9sdbeJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -493.2788042134681,
			"y": -164.69050934587688,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 28.03677090824658,
			"height": 35.05524652174972,
			"seed": 1575171459,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"startBinding": {
				"focus": -0.9323441397306607,
				"gap": 6.962562305354936,
				"elementId": "KjtrBGEa"
			},
			"endBinding": {
				"focus": -0.9316926191820366,
				"gap": 3.6283573976887737,
				"elementId": "ew0dzhpR"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					28.03677090824658,
					35.05524652174972
				]
			]
		},
		{
			"type": "rectangle",
			"version": 1349,
			"versionNonce": 376583732,
			"isDeleted": false,
			"id": "aYhVkM9Nbia228FHhgRVd",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -93.38757597105871,
			"y": -483.7251313777829,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 186.3975514536841,
			"height": 110.36435107920101,
			"seed": 1295456301,
			"groupIds": [
				"wqOiWzisBtzsEiM1rYfPt"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 1280,
			"versionNonce": 1771730700,
			"isDeleted": false,
			"id": "M6rH8Wf4issRiaL9f9cpU",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -93.14667423316871,
			"y": -483.4875654113466,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 394759459,
			"groupIds": [
				"wqOiWzisBtzsEiM1rYfPt"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1149,
			"versionNonce": 1823821748,
			"isDeleted": false,
			"id": "p4FEsScv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -82.36941908411984,
			"y": -478.00889046499447,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 165.69268798828125,
			"height": 96,
			"seed": 811436685,
			"groupIds": [
				"wqOiWzisBtzsEiM1rYfPt"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 20.144310294391317,
			"fontFamily": 2,
			"text": "Number {\n  cell: Cell,\n  value: Option<F>\n}",
			"rawText": "Number {\n  cell: Cell,\n  value: Option<F>\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Number {\n  cell: Cell,\n  value: Option<F>\n}"
		},
		{
			"type": "text",
			"version": 5461,
			"versionNonce": 938934284,
			"isDeleted": false,
			"id": "BssSKIDJ",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -361.5275024633273,
			"y": -465.43030082701887,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 189.8828125,
			"height": 76.8,
			"seed": 905965763,
			"groupIds": [
				"uoeEVQ3PvmOUwRR-EkBZm"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526688637,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl Chip for FieldChip {\n  type config = FieldConfig;\n  fn loaded\n}",
			"rawText": "impl Chip for FieldChip {\n  type config = FieldConfig;\n  fn loaded\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl Chip for FieldChip {\n  type config = FieldConfig;\n  fn loaded\n}"
		},
		{
			"type": "rectangle",
			"version": 3552,
			"versionNonce": 240728372,
			"isDeleted": false,
			"id": "vCS8ua3ce6ZUNHN5Xxyan",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -368.8448650491755,
			"y": -477.4176321552751,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 203.70131960351392,
			"height": 96.15522566171003,
			"seed": 516337901,
			"groupIds": [
				"uoeEVQ3PvmOUwRR-EkBZm"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 451,
			"versionNonce": 165225484,
			"isDeleted": false,
			"id": "QLRAGNiq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -371.9688701712397,
			"y": -498.45171278682267,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 176,
			"height": 19.2,
			"seed": 451600483,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "芯片存储它们自己的配置",
			"rawText": "芯片存储它们自己的配置",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "芯片存储它们自己的配置"
		},
		{
			"type": "text",
			"version": 6404,
			"versionNonce": 1992553140,
			"isDeleted": false,
			"id": "zYwpRRXv",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": 115.87868122685211,
			"y": -470.69225739045726,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 284.5390625,
			"height": 153.6,
			"seed": 100404045,
			"groupIds": [
				"TGTQEilPPWNO7Jq2rt04w",
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl NumericInstructions for FieldChip { \n  type Num = Number<F>;\n  fn load_private() { ... }\n  fn load_constant() { ... }\n  fn expose_public() { ...}\n\n  fn mul()\n}",
			"rawText": "impl NumericInstructions for FieldChip { \n  type Num = Number<F>;\n  fn load_private() { ... }\n  fn load_constant() { ... }\n  fn expose_public() { ...}\n\n  fn mul()\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl NumericInstructions for FieldChip { \n  type Num = Number<F>;\n  fn load_private() { ... }\n  fn load_constant() { ... }\n  fn expose_public() { ...}\n\n  fn mul()\n}"
		},
		{
			"type": "rectangle",
			"version": 4279,
			"versionNonce": 1034994316,
			"isDeleted": false,
			"id": "IeNxLuIrZoxOWorWXa6Wz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 108.32533832297088,
			"y": -483.6673559504984,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 296.6591212694592,
			"height": 170.41149897138553,
			"seed": 1880546307,
			"groupIds": [
				"TGTQEilPPWNO7Jq2rt04w",
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 858,
			"versionNonce": 1151040564,
			"isDeleted": false,
			"id": "fbdGsxS4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 198.54484983649252,
			"y": -355.5721531393805,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 160.1171875,
			"height": 19.2,
			"seed": 1716984237,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "layouter.assign_region",
			"rawText": "layouter.assign_region",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "layouter.assign_region"
		},
		{
			"type": "text",
			"version": 6528,
			"versionNonce": 34640140,
			"isDeleted": false,
			"id": "NA9bi0GI",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": 319.4516835087576,
			"y": -470.0352586934424,
			"strokeColor": "#087f5b",
			"backgroundColor": "#fff",
			"width": 67.5859375,
			"height": 19.2,
			"seed": 562193315,
			"groupIds": [
				"Y4x6sWZEeKCtGh5dqrqKp",
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "FieldChip",
			"rawText": "FieldChip",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FieldChip"
		},
		{
			"type": "text",
			"version": 962,
			"versionNonce": 1371919796,
			"isDeleted": false,
			"id": "jwI34B1N",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 234.89923774113402,
			"y": -313.7383451402286,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 256.3671875,
			"height": 19.2,
			"seed": 2030132237,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let rhs = region.assign_advice(\"rhs\")",
			"rawText": "let rhs = region.assign_advice(\"rhs\")",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let rhs = region.assign_advice(\"rhs\")"
		},
		{
			"type": "text",
			"version": 903,
			"versionNonce": 1987219340,
			"isDeleted": false,
			"id": "1FXiQWwe",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 234.33069048254913,
			"y": -273.3513174050081,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 239.2421875,
			"height": 19.2,
			"seed": 391903043,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "region.constrain_equal(a.cell, lhs)",
			"rawText": "region.constrain_equal(a.cell, lhs)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "region.constrain_equal(a.cell, lhs)"
		},
		{
			"type": "text",
			"version": 715,
			"versionNonce": 1265172276,
			"isDeleted": false,
			"id": "dsBy4KS2",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 235.92681993177086,
			"y": -147.32737983030108,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 249.453125,
			"height": 19.2,
			"seed": 368333421,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "out = Some(Number { cell, value });",
			"rawText": "out = Some(Number { cell, value });",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "out = Some(Number { cell, value });"
		},
		{
			"type": "arrow",
			"version": 1201,
			"versionNonce": 990952972,
			"isDeleted": false,
			"id": "qXk0tYkBWWvWczHuONkKp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 179.1357872739394,
			"y": -343.8430824533757,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 17.371542204171874,
			"height": 0.4575437562601792,
			"seed": 805443299,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"focus": 0.04191585423269181,
				"gap": 2.0375203583812436,
				"elementId": "fbdGsxS4"
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					17.371542204171874,
					-0.4575437562601792
				]
			]
		},
		{
			"type": "text",
			"version": 996,
			"versionNonce": 1240135860,
			"isDeleted": false,
			"id": "GhO4mrnc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 234.85468625369856,
			"y": -293.3301753939144,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 254.59375,
			"height": 19.2,
			"seed": 1509522637,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let lhs = region.assign_advice(\"rhs\")",
			"rawText": "let lhs = region.assign_advice(\"rhs\")",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let lhs = region.assign_advice(\"rhs\")"
		},
		{
			"type": "text",
			"version": 929,
			"versionNonce": 1491410060,
			"isDeleted": false,
			"id": "6SYPEEq8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 234.8948282535565,
			"y": -253.00543489930863,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 241.015625,
			"height": 19.2,
			"seed": 2047033987,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "region.constrain_equal(b.cell, rhs)",
			"rawText": "region.constrain_equal(b.cell, rhs)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "region.constrain_equal(b.cell, rhs)"
		},
		{
			"type": "line",
			"version": 1674,
			"versionNonce": 1760674356,
			"isDeleted": false,
			"id": "4K6TxlB3w41xXQeG2X5Ue",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 1.5918773690470198,
			"x": 343.856084866842,
			"y": -421.27227736013026,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 20.581793138416007,
			"height": 185.15463780834054,
			"seed": 212491053,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495170,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					-7.2771264078329265,
					14.721706539544389
				],
				[
					-9.042199435030064,
					77.76748004570305
				],
				[
					-18.175559913789844,
					91.08808118642123
				],
				[
					-7.833156483354289,
					101.93440253165991
				],
				[
					-5.858291042709599,
					168.4366702590247
				],
				[
					2.406233224626163,
					185.15463780834054
				]
			]
		},
		{
			"type": "text",
			"version": 634,
			"versionNonce": 698985228,
			"isDeleted": false,
			"id": "IVTDtjnM",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 235.13394464382316,
			"y": -226.0693569150676,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 389.75,
			"height": 19.2,
			"seed": 773716515,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let value = a.value.and_then(|a| b.value.map(|b| a * b));",
			"rawText": "let value = a.value.and_then(|a| b.value.map(|b| a * b));",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let value = a.value.and_then(|a| b.value.map(|b| a * b));"
		},
		{
			"type": "text",
			"version": 739,
			"versionNonce": 1600347060,
			"isDeleted": false,
			"id": "8eWh79tf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 235.4953743423588,
			"y": -196.56964139143008,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 310.0390625,
			"height": 38.4,
			"seed": 1181773197,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let cell = region.assign_advice( || \"lhs * rhs\",\n    config.advice[0], 1, value ),",
			"rawText": "let cell = region.assign_advice( || \"lhs * rhs\",\n    config.advice[0], 1, value ),",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let cell = region.assign_advice( || \"lhs * rhs\",\n    config.advice[0], 1, value ),"
		},
		{
			"type": "line",
			"version": 965,
			"versionNonce": 1071147404,
			"isDeleted": false,
			"id": "OQSqpH7m2G4_fgCNgvPU8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 266.25328324475083,
			"y": -179.9433887897908,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 187.689506415991,
			"height": 90.04271273493987,
			"seed": 1765850563,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					-47.60412936984858,
					42.91842633021702
				],
				[
					15.995386171558721,
					90.04271273493987
				],
				[
					109.34912440202584,
					74.85065464437821
				],
				[
					140.0853770461424,
					52.559369789786615
				]
			]
		},
		{
			"type": "line",
			"version": 866,
			"versionNonce": 392545588,
			"isDeleted": false,
			"id": "PRSl1xUS2A0S48ibrlMq3",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 267.96506703696525,
			"y": -209.55131850563293,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 252.34529714803193,
			"height": 140.57823725094454,
			"seed": 1451570157,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					-70.39696377222253,
					63.67839061996108
				],
				[
					-12.491500876344162,
					140.57823725094454
				],
				[
					132.09961922586513,
					132.10626204405025
				],
				[
					181.9483333758094,
					83.91241481232373
				]
			]
		},
		{
			"type": "line",
			"version": 953,
			"versionNonce": 1319852044,
			"isDeleted": false,
			"id": "RyTL-Udl8ymDQtXwHLWZH",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 283.6639922848159,
			"y": -209.06394365827157,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 124.09972025475417,
			"height": 36.300418064371456,
			"seed": 646581603,
			"groupIds": [
				"8eEOTLvtbr0yjqi_dZX2a"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					93.53987133800183,
					9.508791637186619
				],
				[
					124.09972025475417,
					36.300418064371456
				]
			]
		},
		{
			"type": "rectangle",
			"version": 1209,
			"versionNonce": 1180630708,
			"isDeleted": false,
			"id": "BPiByhhb8txZ5Tkdorp7p",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -560.6755796507392,
			"y": -99.79762996112413,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 192.30043723859978,
			"height": 131.86315880241074,
			"seed": 1760702029,
			"groupIds": [
				"veCPugIyAS-hXC1Dupa30"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 990,
			"versionNonce": 303855244,
			"isDeleted": false,
			"id": "i4V80znxbOGYiOMKsjCyR",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -560.4346779128488,
			"y": -99.56006399468788,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 2034788611,
			"groupIds": [
				"veCPugIyAS-hXC1Dupa30"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 965,
			"versionNonce": 83167284,
			"isDeleted": false,
			"id": "Jk05BIfN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -547.8060613611111,
			"y": -92.79366607016482,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 145.53305053710938,
			"height": 120,
			"seed": 1821433005,
			"groupIds": [
				"veCPugIyAS-hXC1Dupa30"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 20.144310294391317,
			"fontFamily": 2,
			"text": "MyCircuit {\n    constant: F,\n    a: Option<F>,\n    b: Option<F>,\n}",
			"rawText": "MyCircuit {\n    constant: F,\n    a: Option<F>,\n    b: Option<F>,\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "MyCircuit {\n    constant: F,\n    a: Option<F>,\n    b: Option<F>,\n}"
		},
		{
			"type": "text",
			"version": 6254,
			"versionNonce": 921027852,
			"isDeleted": false,
			"id": "cNvLNTho",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -344.58441411825777,
			"y": -84.81882947206464,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 306.7421875,
			"height": 211.2,
			"seed": 117067939,
			"groupIds": [
				"cBaG4h086_U6sTEc9QMWe",
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl<F: FieldExt> Circuit<F> for MyCircuit {\n   type Config = FieldConfig;\n   type FloorPlanner = SimpleFloorPlanner;\n\n\n   fn configure()\n\n\n\n\n   fn synthesize",
			"rawText": "impl<F: FieldExt> Circuit<F> for MyCircuit {\n   type Config = FieldConfig;\n   type FloorPlanner = SimpleFloorPlanner;\n\n\n   fn configure()\n\n\n\n\n   fn synthesize",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl<F: FieldExt> Circuit<F> for MyCircuit {\n   type Config = FieldConfig;\n   type FloorPlanner = SimpleFloorPlanner;\n\n\n   fn configure()\n\n\n\n\n   fn synthesize"
		},
		{
			"type": "rectangle",
			"version": 4399,
			"versionNonce": 1728962996,
			"isDeleted": false,
			"id": "vZ_LaSg692WcLOrDcLRmH",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -351.39158986752875,
			"y": -97.07570628334429,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 671.9139687766923,
			"height": 298.05366858183,
			"seed": 957005581,
			"groupIds": [
				"cBaG4h086_U6sTEc9QMWe",
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 686,
			"versionNonce": 1115767692,
			"isDeleted": false,
			"id": "QgpSFcaI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -218.23424613986572,
			"y": -18.293090513546986,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 422.8515625,
			"height": 76.8,
			"seed": 1095648323,
			"groupIds": [
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let advice = [meta.advice_column(), meta.advice_column()];\nlet instance = meta.instance_column();\nlet constant = meta.fixed_column();\nFieldChip::configure(meta, advice, instance, constant)",
			"rawText": "let advice = [meta.advice_column(), meta.advice_column()];\nlet instance = meta.instance_column();\nlet constant = meta.fixed_column();\nFieldChip::configure(meta, advice, instance, constant)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let advice = [meta.advice_column(), meta.advice_column()];\nlet instance = meta.instance_column();\nlet constant = meta.fixed_column();\nFieldChip::configure(meta, advice, instance, constant)"
		},
		{
			"type": "line",
			"version": 1867,
			"versionNonce": 332368692,
			"isDeleted": false,
			"id": "GQ0hOXx-OxD7au8ym1Wdo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0.006426241890897799,
			"x": -222.79072709510956,
			"y": -5.961009422077268,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 11.765720855421243,
			"height": 58.328098811571266,
			"seed": 1960400237,
			"groupIds": [
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					-4.160018389474784,
					4.637686443492474
				],
				[
					-5.1690342894875245,
					24.498599193221686
				],
				[
					-10.390181404431969,
					28.694904231875913
				],
				[
					-4.477876732127752,
					32.111752498037134
				],
				[
					-3.3489315840845375,
					53.06154284145011
				],
				[
					1.3755394509892733,
					58.328098811571266
				]
			]
		},
		{
			"type": "line",
			"version": 2168,
			"versionNonce": 2066280972,
			"isDeleted": false,
			"id": "nZBVO-TuWFmU_BzHOR004",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0.006426241890897799,
			"x": -221.71341092793477,
			"y": 71.21939009462926,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 16.536752381708077,
			"height": 117.91490462759508,
			"seed": 1612891107,
			"groupIds": [
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					-5.537258869856725,
					9.37545309754927
				],
				[
					-5.854072220480134,
					41.21415177927042
				],
				[
					-14.70581864230462,
					48.65178179986901
				],
				[
					-6.01251891579841,
					56.798364027122716
				],
				[
					-4.457648832859286,
					107.268141616531
				],
				[
					1.8309337394034548,
					117.91490462759508
				]
			]
		},
		{
			"type": "text",
			"version": 945,
			"versionNonce": 1570768052,
			"isDeleted": false,
			"id": "NlzZ9f97",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -219.208967111349,
			"y": 62.901558773515035,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 535.96875,
			"height": 134.4,
			"seed": 994451405,
			"groupIds": [
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": " // load Private/Constant values :\n let a = field_chip.load_private(layouter.namespace(|| \"load a\"), self.a)?; \n let b = field_chip.load_private(layouter.namespace(|| \"load b\"), self.b)?;\n let ab = field_chip.mul(layouter.namespace(|| \"a * b\"), a, b)?;\n let absq = field_chip.mul(layouter.namespace(|| \"ab * ab\"), ab.clone(), ab)?;\n // 将结果作为电路的公开输入进行公开\n field_chip.expose_public(layouter.namespace(|| \"expose c\"), c, 0)",
			"rawText": " // load Private/Constant values :\n let a = field_chip.load_private(layouter.namespace(|| \"load a\"), self.a)?; \n let b = field_chip.load_private(layouter.namespace(|| \"load b\"), self.b)?;\n let ab = field_chip.mul(layouter.namespace(|| \"a * b\"), a, b)?;\n let absq = field_chip.mul(layouter.namespace(|| \"ab * ab\"), ab.clone(), ab)?;\n // 将结果作为电路的公开输入进行公开\n field_chip.expose_public(layouter.namespace(|| \"expose c\"), c, 0)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": " // load Private/Constant values :\n let a = field_chip.load_private(layouter.namespace(|| \"load a\"), self.a)?; \n let b = field_chip.load_private(layouter.namespace(|| \"load b\"), self.b)?;\n let ab = field_chip.mul(layouter.namespace(|| \"a * b\"), a, b)?;\n let absq = field_chip.mul(layouter.namespace(|| \"ab * ab\"), ab.clone(), ab)?;\n // 将结果作为电路的公开输入进行公开\n field_chip.expose_public(layouter.namespace(|| \"expose c\"), c, 0)"
		},
		{
			"type": "text",
			"version": 419,
			"versionNonce": 1339478156,
			"isDeleted": false,
			"id": "4v6OryrH",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -219.25756135175743,
			"y": 62.69764856734287,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 230.34375,
			"height": 19.2,
			"seed": 461787011,
			"groupIds": [
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": " // load Private/Constant values :",
			"rawText": " // load Private/Constant values :",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": " // load Private/Constant values :"
		},
		{
			"type": "text",
			"version": 371,
			"versionNonce": 1512394292,
			"isDeleted": false,
			"id": "51MS8lu4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -219.09516049634976,
			"y": 159.01350113765443,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 273.78125,
			"height": 19.2,
			"seed": 40541741,
			"groupIds": [
				"FL50l7ZUxrBTZ2T62_01q"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": " // 将结果作为电路的公开输入进行公开",
			"rawText": " // 将结果作为电路的公开输入进行公开",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": " // 将结果作为电路的公开输入进行公开"
		},
		{
			"type": "rectangle",
			"version": 88,
			"versionNonce": 1078255372,
			"isDeleted": false,
			"id": "56pd4DrA-JRfeKV5SXlQm",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -580.9961679435988,
			"y": -360.7013619944763,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 541.7769719781616,
			"height": 129.85130299411173,
			"seed": 632776483,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 304,
			"versionNonce": 2051368884,
			"isDeleted": false,
			"id": "H88Z2lP_4Mzz6BhnC_rnA",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -254.9867195622628,
			"y": -231.57687145605337,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 69.09782477733802,
			"height": 172.86633957241384,
			"seed": 1093685389,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					67.4584525001369,
					82.0877082891999
				],
				[
					69.09782477733802,
					172.86633957241384
				]
			]
		},
		{
			"type": "rectangle",
			"version": 61,
			"versionNonce": 1156390284,
			"isDeleted": false,
			"id": "gD6FMHFgyFVnvSRl-PI2S",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -582.7533930911912,
			"y": -501.93189443900985,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 450.8680303977285,
			"height": 131.21055502537024,
			"seed": 1696893635,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 197,
			"versionNonce": 572936500,
			"isDeleted": false,
			"id": "t_zTQFlnq-1-n74bpgUXK",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -235.39226310220022,
			"y": -502.51936891879575,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 589.6267075974758,
			"height": 68.87741652580019,
			"seed": 1431353069,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					72.02476278140148,
					-33.20248528302068
				],
				[
					515.9573943302566,
					-30.51329756075279
				],
				[
					589.6267075974758,
					35.67493124277951
				]
			]
		},
		{
			"type": "line",
			"version": 121,
			"versionNonce": 923372556,
			"isDeleted": false,
			"id": "6jdiYU_1IYW1W6CxGb7Hi",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -236.57564052364353,
			"y": -44.24357803504063,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 86.46382828351454,
			"height": 0.28543347512481887,
			"seed": 1852586595,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					86.46382828351454,
					-0.28543347512481887
				]
			]
		},
		{
			"type": "line",
			"version": 111,
			"versionNonce": 1093536436,
			"isDeleted": false,
			"id": "uOvlbZ7juIJZQ-1PoK4lQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -553.3982344296254,
			"y": -321.7435069709247,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 101.88240047687259,
			"height": 0.6250439520910049,
			"seed": 693457229,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					101.88240047687259,
					0.6250439520910049
				]
			]
		},
		{
			"type": "line",
			"version": 57,
			"versionNonce": 949234316,
			"isDeleted": false,
			"id": "vfWQOL2dgb0zdP4hraLgo",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -555.3166066219486,
			"y": -453.62086105821777,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 89.66162159784028,
			"height": 0.31027725914691473,
			"seed": 1046279683,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495171,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					89.66162159784028,
					0.31027725914691473
				]
			]
		},
		{
			"type": "line",
			"version": 36,
			"versionNonce": 1778553908,
			"isDeleted": false,
			"id": "IVkDM0CrN82MRbOmtKrNj",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 314.57944453774337,
			"y": -447.5173239123926,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 70.06257641562593,
			"height": 1.1876746478551468,
			"seed": 446144429,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					70.06257641562593,
					1.1876746478551468
				]
			]
		},
		{
			"type": "rectangle",
			"version": 94,
			"versionNonce": 474413324,
			"isDeleted": false,
			"id": "sN4cYob3SsDVmszk8AB32",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 90,
			"angle": 0,
			"x": -119.62456818606199,
			"y": -499.81198167670595,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 624.3681233019224,
			"height": 190.55132808456767,
			"seed": 871710115,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694526495172,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 495,
			"versionNonce": 1787424180,
			"isDeleted": false,
			"id": "vWilajeBLAj72YMSaea49",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 90,
			"angle": 0,
			"x": 126.14252340434984,
			"y": -308.2304724374428,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 97.91730320049692,
			"height": 360.06429955666545,
			"seed": 524362253,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					97.91730320049692,
					297.00420444209453
				],
				[
					36.72360297700584,
					360.06429955666545
				]
			]
		},
		{
			"type": "line",
			"version": 54,
			"versionNonce": 1302067084,
			"isDeleted": false,
			"id": "DXMWj08uAK8QC-bVSCY4k",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 90,
			"angle": 0,
			"x": -222.75232802517587,
			"y": 59.97301483018123,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 382.56702170530593,
			"height": 0.3847620446288147,
			"seed": 1248039235,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					382.56702170530593,
					0.3847620446288147
				]
			]
		},
		{
			"type": "text",
			"version": 5679,
			"versionNonce": 1676285748,
			"isDeleted": false,
			"id": "7hhy6ZXu",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -328.59317239581867,
			"y": -465.93892412479465,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fff",
			"width": 32.90625,
			"height": 19.2,
			"seed": 743135341,
			"groupIds": [
				"CUfCVfa4M3N0uHYWdL-Hv"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Chip",
			"rawText": "Chip",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Chip"
		},
		{
			"type": "text",
			"version": 439,
			"versionNonce": 952968716,
			"isDeleted": false,
			"id": "WZrUB9ss",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -683.6159673855633,
			"y": -183.1505782369028,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 141.375,
			"height": 19.2,
			"seed": 319189219,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "NumericInstructions",
			"rawText": "NumericInstructions",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "NumericInstructions"
		},
		{
			"type": "rectangle",
			"version": 368,
			"versionNonce": 1653149876,
			"isDeleted": false,
			"id": "JbaeQa-rNVs6SZNNY_4r1",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -746.4786844902692,
			"y": 313.7531742119937,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 228.00408614309194,
			"height": 35.62634919819072,
			"seed": 385402292,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 354,
			"versionNonce": 852788364,
			"isDeleted": false,
			"id": "WqjNCZek",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -741.6251688652692,
			"y": 322.6492088542635,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 214.2890625,
			"height": 19.2,
			"seed": 103194508,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "GV_PTU2vc5lmi5YJGP3Sw",
					"type": "arrow"
				},
				{
					"id": "wAcaAaskzUcd2-8eS_onb",
					"type": "arrow"
				},
				{
					"id": "HLhYG9Jyi2lUJQfVfZRB_",
					"type": "arrow"
				},
				{
					"id": "XWeKc0dB0VtP4qz1djtWx",
					"type": "arrow"
				},
				{
					"id": "iZzIbgQcHgDOxiwtRUbfj",
					"type": "arrow"
				}
			],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "trait NumericInstructions: Chip",
			"rawText": "trait NumericInstructions: Chip",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "trait NumericInstructions: Chip"
		},
		{
			"type": "text",
			"version": 300,
			"versionNonce": 1980553780,
			"isDeleted": false,
			"id": "VqfswGJL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -741.6889942000907,
			"y": 322.09045417949085,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 26.671875,
			"height": 19.2,
			"seed": 1287515956,
			"groupIds": [],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "trait",
			"rawText": "trait",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "trait"
		},
		{
			"type": "arrow",
			"version": 1039,
			"versionNonce": 1634472716,
			"isDeleted": false,
			"id": "GV_PTU2vc5lmi5YJGP3Sw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -521.6959481380156,
			"y": 319.7139280289781,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 29.41856284071116,
			"height": 14.779215115203556,
			"seed": 1505938956,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WqjNCZek",
				"focus": 0.6956847505976528,
				"gap": 5.640158227253551
			},
			"endBinding": {
				"elementId": "DuSeLb2Z",
				"focus": 1.1829792192197437,
				"gap": 4.991886407756169
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					29.41856284071116,
					-14.779215115203556
				]
			]
		},
		{
			"type": "text",
			"version": 353,
			"versionNonce": 1230730164,
			"isDeleted": false,
			"id": "S7BOTS0N",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -487.0289016080469,
			"y": 288.4415779155212,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 72.9140625,
			"height": 19.2,
			"seed": 857860276,
			"groupIds": [],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "type Num;",
			"rawText": "type Num;",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "type Num;"
		},
		{
			"type": "arrow",
			"version": 1583,
			"versionNonce": 588108172,
			"isDeleted": false,
			"id": "wAcaAaskzUcd2-8eS_onb",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -520.4551696754293,
			"y": 320.3016623824766,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 25.18176725233525,
			"height": 1.0231282479757056,
			"seed": 1993682060,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WqjNCZek",
				"focus": -1.1882799002836968,
				"gap": 6.880936689839928
			},
			"endBinding": {
				"elementId": "MyCjQPRl",
				"focus": 1.15275612759286,
				"gap": 7.79805551968775
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					25.18176725233525,
					1.0231282479757056
				]
			]
		},
		{
			"type": "text",
			"version": 585,
			"versionNonce": 983227700,
			"isDeleted": false,
			"id": "DuSeLb2Z",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -487.2854988895483,
			"y": 309.0077489422764,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 104.9609375,
			"height": 19.2,
			"seed": 1842000436,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "GV_PTU2vc5lmi5YJGP3Sw",
					"type": "arrow"
				}
			],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn load_private",
			"rawText": "fn load_private",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn load_private"
		},
		{
			"type": "arrow",
			"version": 744,
			"versionNonce": 1995685900,
			"isDeleted": false,
			"id": "HLhYG9Jyi2lUJQfVfZRB_",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -519.775151977346,
			"y": 335.0980448443812,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 27.794787099778887,
			"height": 3.4086045337234054,
			"seed": 790084364,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WqjNCZek",
				"focus": -0.49332514549489925,
				"gap": 7.560954387923175
			},
			"endBinding": {
				"elementId": "vB8pIC14",
				"focus": 0.35004534075035326,
				"gap": 8.890591833920467
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					27.794787099778887,
					3.4086045337234054
				]
			]
		},
		{
			"type": "text",
			"version": 366,
			"versionNonce": 1932596916,
			"isDeleted": false,
			"id": "MyCjQPRl",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -487.47534690340626,
			"y": 328.2432865133543,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 117.421875,
			"height": 19.2,
			"seed": 1000229812,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "wAcaAaskzUcd2-8eS_onb",
					"type": "arrow"
				}
			],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn load_constant",
			"rawText": "fn load_constant",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn load_constant"
		},
		{
			"type": "text",
			"version": 338,
			"versionNonce": 1696063116,
			"isDeleted": false,
			"id": "vB8pIC14",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -487.7618373990547,
			"y": 347.39724121202505,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 176.5234375,
			"height": 19.2,
			"seed": 218397068,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "HLhYG9Jyi2lUJQfVfZRB_",
					"type": "arrow"
				}
			],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn mul // Returns c = a*b ",
			"rawText": "fn mul // Returns c = a*b ",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn mul // Returns c = a*b "
		},
		{
			"type": "arrow",
			"version": 786,
			"versionNonce": 2143559732,
			"isDeleted": false,
			"id": "XWeKc0dB0VtP4qz1djtWx",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -520.9906235175245,
			"y": 338.66889965204416,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 27.80502439300369,
			"height": 19.782646113073724,
			"seed": 247641396,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WqjNCZek",
				"focus": -0.8659571116988269,
				"gap": 6.345482847744734
			},
			"endBinding": {
				"elementId": "86lLPp3D",
				"focus": -0.5258611347222364,
				"gap": 8.791987742245311
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					27.80502439300369,
					19.782646113073724
				]
			]
		},
		{
			"type": "text",
			"version": 282,
			"versionNonce": 1714695436,
			"isDeleted": false,
			"id": "86lLPp3D",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -488.7084157539789,
			"y": 367.24353350736317,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 120.0859375,
			"height": 19.2,
			"seed": 1003841548,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "XWeKc0dB0VtP4qz1djtWx",
					"type": "arrow"
				},
				{
					"id": "iZzIbgQcHgDOxiwtRUbfj",
					"type": "arrow"
				}
			],
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "fn expose_public",
			"rawText": "fn expose_public",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fn expose_public"
		},
		{
			"type": "arrow",
			"version": 776,
			"versionNonce": 1748629940,
			"isDeleted": false,
			"id": "iZzIbgQcHgDOxiwtRUbfj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -520.3735440599144,
			"y": 341.0678076768469,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 28.03677090824658,
			"height": 35.05524652174972,
			"seed": 614330036,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526495172,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WqjNCZek",
				"focus": -0.9323441397306598,
				"gap": 6.9625623053548225
			},
			"endBinding": {
				"elementId": "86lLPp3D",
				"focus": -0.9316926191820384,
				"gap": 3.6283573976888874
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					28.03677090824658,
					35.05524652174972
				]
			]
		},
		{
			"type": "text",
			"version": 461,
			"versionNonce": 1271049100,
			"isDeleted": false,
			"id": "PCtEg9Yq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -710.7107072320096,
			"y": 322.60773878582097,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 141.375,
			"height": 19.2,
			"seed": 1870956172,
			"groupIds": [],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526495173,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "NumericInstructions",
			"rawText": "NumericInstructions",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "NumericInstructions"
		},
		{
			"id": "AaQqzYtVJ0fMH9c56E9S5",
			"type": "rectangle",
			"x": -762.3094015599033,
			"y": 273.9586416973375,
			"width": 542.4550784496479,
			"height": 121.37401057348598,
			"angle": 0,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"seed": 987646604,
			"version": 110,
			"versionNonce": 1203282740,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694526495173,
			"link": null,
			"locked": false
		},
		{
			"id": "2FtvK7hx",
			"type": "text",
			"x": -322.28771492005114,
			"y": 276.71514655383555,
			"width": 84,
			"height": 33.6,
			"angle": 0,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": null,
			"seed": 1929213068,
			"version": 95,
			"versionNonce": 449703052,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694527193962,
			"link": null,
			"locked": false,
			"text": "指令集",
			"rawText": "指令集",
			"fontSize": 28,
			"fontFamily": 4,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "指令集"
		},
		{
			"type": "rectangle",
			"version": 618,
			"versionNonce": 1049902132,
			"isDeleted": false,
			"id": "oCBMjhi0BHDj8bgivG8JH",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -739.8057475192293,
			"y": 439.8004541143398,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 177.44967413552595,
			"height": 107.17786697419095,
			"seed": 639570700,
			"groupIds": [
				"hXb_TzThdpsP_sBebblb8"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526514793,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 568,
			"versionNonce": 1359204620,
			"isDeleted": false,
			"id": "rYTS8zEHtvtKH_UANeIEQ",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -739.5648457813393,
			"y": 440.03802008077605,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 492336052,
			"groupIds": [
				"hXb_TzThdpsP_sBebblb8"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": null,
			"updated": 1694526514793,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 307,
			"versionNonce": 245798324,
			"isDeleted": false,
			"id": "f4LpiJl2",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -728.5382585913362,
			"y": 445.64160987943,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 91.162109375,
			"height": 96,
			"seed": 1499517324,
			"groupIds": [
				"hXb_TzThdpsP_sBebblb8"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526514793,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "FieldChip{\n  config,\n  _marker\n}",
			"rawText": "FieldChip{\n  config,\n  _marker\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FieldChip{\n  config,\n  _marker\n}"
		},
		{
			"type": "text",
			"version": 5595,
			"versionNonce": 665126708,
			"isDeleted": false,
			"id": "w05hUMu5",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -533.8074299680983,
			"y": 463.94538108685083,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 231.796875,
			"height": 96,
			"seed": 116907316,
			"groupIds": [
				"KJv5Avow8U9KmgMgLRH29"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526772865,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "impl Chip for FieldChip {\n  type config = FieldConfig\n  fn loaded\n}",
			"rawText": "impl Chip for FieldChip {\n  type config = FieldConfig\n  fn loaded\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl Chip for FieldChip {\n  type config = FieldConfig\n  fn loaded\n}"
		},
		{
			"type": "rectangle",
			"version": 3723,
			"versionNonce": 1206346804,
			"isDeleted": false,
			"id": "3g9f9nGHXowyJrdn_4btf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -546.0565834666336,
			"y": 459.40580354844053,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 245.0142334057394,
			"height": 103.3470256887448,
			"seed": 1793507340,
			"groupIds": [
				"KJv5Avow8U9KmgMgLRH29"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526906024,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 810,
			"versionNonce": 361519284,
			"isDeleted": false,
			"id": "q8WArw7a",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -530.9921925087016,
			"y": 422.042201719643,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 263.91778564453125,
			"height": 32.4,
			"seed": 737023668,
			"groupIds": [],
			"roundness": null,
			"boundElements": null,
			"updated": 1694528241093,
			"link": null,
			"locked": false,
			"fontSize": 27.448817890554594,
			"fontFamily": 4,
			"text": "芯片(实现在有限域上)",
			"rawText": "芯片(实现在有限域上)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "芯片(实现在有限域上)"
		},
		{
			"type": "rectangle",
			"version": 248,
			"versionNonce": 529076108,
			"isDeleted": false,
			"id": "2mKv9TIMev9fHoZqoQ_7-",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 80,
			"angle": 0,
			"x": -759.9651115086494,
			"y": 423.79180955822676,
			"strokeColor": "#343a40",
			"backgroundColor": "transparent",
			"width": 536.3317052793765,
			"height": 148.51104851152206,
			"seed": 1655365260,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": null,
			"updated": 1694526793458,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 148,
			"versionNonce": 765484172,
			"isDeleted": false,
			"id": "eU81oI85tO_ayFTrNy2AS",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -732.5283250394068,
			"y": 472.10284293901884,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 89.66162159784028,
			"height": 0.31027725914691473,
			"seed": 417407028,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526514793,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					89.66162159784028,
					0.31027725914691473
				]
			]
		},
		{
			"type": "text",
			"version": 5845,
			"versionNonce": 1168841140,
			"isDeleted": false,
			"id": "7zCLaav0",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -491.4445951974469,
			"y": 464.0864941419561,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fff",
			"width": 41.1328125,
			"height": 24,
			"seed": 963063052,
			"groupIds": [
				"nZqCZ14g2q-i2wnK1ATIH"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526783198,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "Chip",
			"rawText": "Chip",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Chip"
		},
		{
			"type": "text",
			"version": 6074,
			"versionNonce": 1017848500,
			"isDeleted": false,
			"id": "XVr4ox7S",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -403.2369937712243,
			"y": 488.56083557057144,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fff",
			"width": 101.162109375,
			"height": 24,
			"seed": 240884876,
			"groupIds": [
				"m3qS63LzbPjyf1flRdpfl"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "ly04FXtZUIS1QIPyo7p3Z",
					"type": "arrow"
				}
			],
			"updated": 1694526919028,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "FieldConfig",
			"rawText": "FieldConfig",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FieldConfig"
		},
		{
			"id": "Tee8dpGOZA4WiQfII1URX",
			"type": "line",
			"x": -402.2990174661575,
			"y": 514.3647779570897,
			"width": 99.25785095587207,
			"height": 1.1431086127229264,
			"angle": 0,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 80,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"seed": 441178548,
			"version": 191,
			"versionNonce": 1426595764,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694526899119,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					99.25785095587207,
					1.1431086127229264
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": null
		},
		{
			"id": "ly04FXtZUIS1QIPyo7p3Z",
			"type": "arrow",
			"x": -298.87152972857893,
			"y": 501.97906969267433,
			"width": 148.03524039646652,
			"height": 103.2963382442831,
			"angle": 0,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"seed": 1398062516,
			"version": 388,
			"versionNonce": 1951903796,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "vwOvzJz0"
				}
			],
			"updated": 1694528259706,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					68.2942859858365,
					-80.0555523325981
				],
				[
					148.03524039646652,
					-103.2963382442831
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "XVr4ox7S",
				"focus": 0.9042422451815039,
				"gap": 3.2033546676453852
			},
			"endBinding": {
				"elementId": "c3x_ag1jX4sbqOMPvVrvZ",
				"focus": 0.8441372283794563,
				"gap": 1.5586324261088293
			},
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "vwOvzJz0",
			"type": "text",
			"x": -255.20543914971375,
			"y": 445.0733854141178,
			"width": 56,
			"height": 67.2,
			"angle": 0,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": null,
			"seed": 781299764,
			"version": 31,
			"versionNonce": 1133717516,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694528232837,
			"link": null,
			"locked": false,
			"text": "配置\n芯片",
			"rawText": "配置\n芯片",
			"fontSize": 28,
			"fontFamily": 4,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "ly04FXtZUIS1QIPyo7p3Z",
			"originalText": "配置\n芯片"
		},
		{
			"type": "rectangle",
			"version": 747,
			"versionNonce": 1606432948,
			"isDeleted": false,
			"id": "NtMbDo0CI6ohCjL86kWD5",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -131.6888489786728,
			"y": 383.57466788749537,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 177.44967413552595,
			"height": 107.17786697419095,
			"seed": 1395866252,
			"groupIds": [
				"xlvZeiWRaUk22v8cAHknr"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "ly04FXtZUIS1QIPyo7p3Z",
					"type": "arrow"
				}
			],
			"updated": 1694526981667,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 696,
			"versionNonce": 511080756,
			"isDeleted": false,
			"id": "aeL3YniY1-3brbQpmR9Pe",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -131.4479472407828,
			"y": 383.8122338539316,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 412910644,
			"groupIds": [
				"xlvZeiWRaUk22v8cAHknr"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": null,
			"updated": 1694526981318,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 468,
			"versionNonce": 1653385228,
			"isDeleted": false,
			"id": "yqHSWGOl",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -120.54627490308133,
			"y": 389.2909088002838,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 162.294921875,
			"height": 96,
			"seed": 813959436,
			"groupIds": [
				"xlvZeiWRaUk22v8cAHknr"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "_wzrecBL6QM-b41pTqJYa",
					"type": "arrow"
				}
			],
			"updated": 1694527338968,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "FieldConfig {\n  advice, instance,\n  constant, s_mul, \n}",
			"rawText": "FieldConfig {\n  advice, instance,\n  constant, s_mul, \n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FieldConfig {\n  advice, instance,\n  constant, s_mul, \n}"
		},
		{
			"type": "text",
			"version": 5776,
			"versionNonce": 288961204,
			"isDeleted": false,
			"id": "E9ycu1jU",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": 67.70362361029913,
			"y": 412.16250236450423,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 124.5,
			"height": 57.599999999999994,
			"seed": 275111348,
			"groupIds": [
				"ha3tWjZZWP5QffEzWb18j"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526981318,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl FieldConfig {\n  fn configure()\n}",
			"rawText": "impl FieldConfig {\n  fn configure()\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl FieldConfig {\n  fn configure()\n}"
		},
		{
			"type": "rectangle",
			"version": 3776,
			"versionNonce": 539984524,
			"isDeleted": false,
			"id": "a70k9e1Y1CRaeZwg-fWPE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 57.95213812542386,
			"y": 398.4714348298411,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 145.52720563443728,
			"height": 86.47529221462202,
			"seed": 2008483724,
			"groupIds": [
				"ha3tWjZZWP5QffEzWb18j"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526981318,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 948,
			"versionNonce": 637787276,
			"isDeleted": false,
			"id": "aoog6yASgh-USFbk1BLKE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 170.81105981547466,
			"y": 442.0964195342267,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 61.57745033611309,
			"height": 3.6323822146966904,
			"seed": 1221068596,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526981668,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "57wXf6x9",
				"focus": -0.35917948376190395,
				"gap": 1.0000000000001137
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					61.57745033611309,
					3.6323822146966904
				]
			]
		},
		{
			"type": "text",
			"version": 554,
			"versionNonce": 142846260,
			"isDeleted": false,
			"id": "57wXf6x9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 233.38851015158787,
			"y": 422.56650731172874,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 152.109375,
			"height": 38.4,
			"seed": 218510860,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "aoog6yASgh-USFbk1BLKE",
					"type": "arrow"
				},
				{
					"id": "1U4vB0LR9FmzHqS_0lpLO",
					"type": "arrow"
				}
			],
			"updated": 1694527368714,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "meta.enable_equality\nmeta.create_gate",
			"rawText": "meta.enable_equality\nmeta.create_gate",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "meta.enable_equality\nmeta.create_gate"
		},
		{
			"type": "text",
			"version": 351,
			"versionNonce": 1072371124,
			"isDeleted": false,
			"id": "tlxZ3QKN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": 58.50105363228033,
			"y": 374.4334978176463,
			"strokeColor": "#495057",
			"backgroundColor": "transparent",
			"width": 140.4765625,
			"height": 19.2,
			"seed": 1445527732,
			"groupIds": [],
			"roundness": null,
			"boundElements": null,
			"updated": 1694526981318,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "添加约束, 定义 gate",
			"rawText": "添加约束, 定义 gate",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "添加约束, 定义 gate"
		},
		{
			"type": "rectangle",
			"version": 136,
			"versionNonce": 348473140,
			"isDeleted": false,
			"id": "c3x_ag1jX4sbqOMPvVrvZ",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -149.27765690600359,
			"y": 375.80338386155415,
			"strokeColor": "#343a40",
			"backgroundColor": "transparent",
			"width": 541.7769719781616,
			"height": 129.85130299411173,
			"seed": 1030460556,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "ly04FXtZUIS1QIPyo7p3Z",
					"type": "arrow"
				}
			],
			"updated": 1694528237529,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 152,
			"versionNonce": 1122385716,
			"isDeleted": false,
			"id": "mWITt0y5lxS1NBeMboVLi",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -121.67972339203016,
			"y": 414.76123888510574,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 101.88240047687259,
			"height": 0.6250439520910049,
			"seed": 1345243700,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1694526981318,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					101.88240047687259,
					0.6250439520910049
				]
			]
		},
		{
			"id": "BH2hPoz7",
			"type": "text",
			"x": -140.83899486104485,
			"y": 267.2896819167223,
			"width": 443.59490966796875,
			"height": 97.19999999999999,
			"angle": 0,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": null,
			"seed": 37064844,
			"version": 514,
			"versionNonce": 1907995188,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694528356562,
			"link": null,
			"locked": false,
			"text": "配置芯片：\n1. 挑选所需元件\n2. 根据功能逻辑配置其连接排布方式",
			"rawText": "配置芯片：\n1. 挑选所需元件\n2. 根据功能逻辑配置其连接排布方式",
			"fontSize": 27.441742370516398,
			"fontFamily": 4,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "配置芯片：\n1. 挑选所需元件\n2. 根据功能逻辑配置其连接排布方式"
		},
		{
			"type": "text",
			"version": 533,
			"versionNonce": 1319921972,
			"isDeleted": false,
			"id": "n0DxIMUh",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -134.45594991554424,
			"y": 510.9075550352664,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 213.30398559570312,
			"height": 33.6,
			"seed": 1673793076,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "_wzrecBL6QM-b41pTqJYa",
					"type": "arrow"
				}
			],
			"updated": 1694527396010,
			"link": null,
			"locked": false,
			"fontSize": 28,
			"fontFamily": 4,
			"text": "(①选取所需元件)",
			"rawText": "(①选取所需元件)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "(①选取所需元件)"
		},
		{
			"type": "text",
			"version": 660,
			"versionNonce": 2483892,
			"isDeleted": false,
			"id": "MHyuCHw1",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 143.14880425480965,
			"y": 471.8385656224829,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 177.75624084472656,
			"height": 32.4,
			"seed": 76663348,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "1U4vB0LR9FmzHqS_0lpLO",
					"type": "arrow"
				}
			],
			"updated": 1694527378062,
			"link": null,
			"locked": false,
			"fontSize": 27.441742370516398,
			"fontFamily": 4,
			"text": "(② 连接 排布)",
			"rawText": "(② 连接 排布)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "(② 连接 排布)"
		},
		{
			"id": "_wzrecBL6QM-b41pTqJYa",
			"type": "arrow",
			"x": -139.01746248172475,
			"y": 461.45146017247015,
			"width": 39.67935830314792,
			"height": 68.92319063785493,
			"angle": 0,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"seed": 1718697484,
			"version": 74,
			"versionNonce": 1201322036,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694527338973,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-38.12826696511013,
					47.50489393545769
				],
				[
					1.5510913380377929,
					68.92319063785493
				]
			],
			"lastCommittedPoint": [
				1.5510913380377929,
				68.92319063785493
			],
			"startBinding": {
				"elementId": "yqHSWGOl",
				"focus": 0.6703842162302364,
				"gap": 18.471187578643423
			},
			"endBinding": {
				"elementId": "n0DxIMUh",
				"focus": -0.8360189011583498,
				"gap": 3.010421228142718
			},
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "1U4vB0LR9FmzHqS_0lpLO",
			"type": "arrow",
			"x": 231.45589996189005,
			"y": 417.14342403167274,
			"width": 159.72724738202646,
			"height": 85.37679001883606,
			"angle": 0,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"seed": 1586901772,
			"version": 353,
			"versionNonce": 1833544588,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694527386301,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					159.72724738202646,
					-5.064285531194116
				],
				[
					149.58437559190213,
					70.72059319440234
				],
				[
					94.09120804346696,
					80.31250448764195
				]
			],
			"lastCommittedPoint": [
				135.45181581190616,
				76.53566693174861
			],
			"startBinding": {
				"elementId": "57wXf6x9",
				"focus": -1.2537718086671072,
				"gap": 5.423083280056005
			},
			"endBinding": {
				"elementId": "MHyuCHw1",
				"focus": 0.81052601020736,
				"gap": 4.642062905820808
			},
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "m3GTL73Scqs-SB_GAcaKW",
			"type": "line",
			"x": 236.44771413598482,
			"y": 410.74202045714094,
			"width": 184.41041097903235,
			"height": 97.18274581694186,
			"angle": 0,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"seed": 515052812,
			"version": 88,
			"versionNonce": 1804646964,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694527358551,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					175.30611610021822,
					-16.07127610667368
				],
				[
					184.41041097903235,
					81.11146971026818
				],
				[
					132.43253434699318,
					79.38575895987219
				]
			],
			"lastCommittedPoint": [
				132.43253434699318,
				79.38575895987219
			],
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": null
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#0b7285",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 4,
		"currentItemFontSize": 28,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 803.7485996900414,
		"scrollY": 41.41998337069593,
		"zoom": {
			"value": 0.8191819276016112
		},
		"currentItemRoundness": "round",
		"gridSize": null,
		"colorPalette": {},
		"currentStrokeOptions": null,
		"previousGridSize": null
	},
	"files": {}
}
```
%%