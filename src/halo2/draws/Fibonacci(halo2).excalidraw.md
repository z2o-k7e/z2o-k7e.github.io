---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
struct Config {
    elem_1: Column<Advice>,
    elem_2: Column<Advice>,
    elem_3: Column<Advice>,
    q_fib: Selector,
} ^TYH4ImLU

impl Config {
    fn configure(cs: &mut ConstrainSystem)
    


    fn init







    fn assign ^MB5YUU7M

// i = 1, 2, 3 
let elem_i = cs.advice_column();
cs.enable_equality(elem_i);

let q_fib = cs.selector();



cs.create_gate ^LD2RpKyg

let q_fib = virtual_cells.query_selector(q_fib);
let elem_1 = virtual_cells.query_advice(elem_1, Rotation::cur());
let elem_2 = virtual_cells.query_advice(elem_2, Rotation::cur());
let elem_3 = virtual_cells.query_advice(elem_3, Rotation::cur());

return vec![ q_fib * (ele_1 + elem_2 - elem_3) ] ^k1rAieSh

struct MyCircuit<F: Field> {
    elem_1: Value<F>, // 1
    elem_2: Value<F>, // 1
} ^YLtoxDvd

impl<F: Field> Circuit<F> for MyCircuit<F> {
    type Config = Config;
    type FloorPlanner = SimpleFloorPlanner;
    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {
        Self::Config::configure(meta)
    }
    fn synthesize(&self, config: Self::Config, mut layouter: impl Layouter<F>) {
        // return (elem_2 = 1, elem_3 = 2)
        let (elem_2, elem_3) = config.init(layouter.namespace(|| "init"), self.elem_1, self.elem_2)?;
        // 1 + 2 = 3
        config.assign(layouter.namespace(|| "first assign after init"), elem_2, elem_3)?;
    }

 ^VuQEEKsr

configure ^xyfOua3c

configure ^pO7EnjUU

i ^KLpVujyP

i ^kcyw12We

i ^2ctpAHhM

// return q_fib × (ele_1 + elem_2 - elem_3) ^9Ez8j4kU

layouter.assign_region ^L9lPbSZ9

// 初始化 Fibonacci 序列前 2 个数 ^IJKn8nVC

let offset = 0;
// Assign elem_1
region.assign_advice(|| "elem_1", self.elem_1, offset, || elem_1)?;
let elem_2 = region.assign_advice(|| "elem_2", self.elem_2, offset, || elem_2)?;
let e3_val = elem_1 + elem_2.value_field().evaluate();
let elem_3 = region.assign_advice(|| "elem_3", self.elem_3, offset, || e3_val)?
return OK((elem_2, elem_3)) ^LMy2Slkf

init ^ivCyMwH1

assign ^TXaNMrwG

layouter.assign_region ^3rw2zltJ

let offset = 0;
// Copy elem_1 (which is the previous elem_2)
let elem_1 = elem_2.copy_advice(|| "copy elem_2 into current elem_1", &mut region, self.elem_1, offset)?;
// Copy elem_2 (which is the previous elem_3)
let elem_2 = elem_3.copy_advice(|| "copy elem_3 into current elem_2", &mut region, self.elem_2, offset)?;
let e3_val = elem_1.value_field().evaluate() + elem_2.value_field().evaluate();
// Assign elem_3
let elem_3 = region.assign_advice(|| "elem_3", self.elem_3, offset, || e3_val)?;
 ^YDQePEi5

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.8.17",
	"elements": [
		{
			"type": "text",
			"version": 7321,
			"versionNonce": 2099462494,
			"isDeleted": false,
			"id": "MB5YUU7M",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -838.2240023014224,
			"y": 641.6554946811682,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 300.546875,
			"height": 268.8,
			"seed": 1449811458,
			"groupIds": [
				"QuJ6k9wmCCsZlezQhd1cM",
				"LsOdPlZlexa5Q8O6BgNyf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974457422,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl Config {\n    fn configure(cs: &mut ConstrainSystem)\n    \n\n\n    fn init\n\n\n\n\n\n\n\n    fn assign",
			"rawText": "impl Config {\n    fn configure(cs: &mut ConstrainSystem)\n    \n\n\n    fn init\n\n\n\n\n\n\n\n    fn assign",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl Config {\n    fn configure(cs: &mut ConstrainSystem)\n    \n\n\n    fn init\n\n\n\n\n\n\n\n    fn assign"
		},
		{
			"type": "rectangle",
			"version": 5252,
			"versionNonce": 779187842,
			"isDeleted": false,
			"id": "TPDvWtovmHJD0Q0HZqxin",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -847.7025751409143,
			"y": 628.7552599763649,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 323.7759313751207,
			"height": 344.96424175875893,
			"seed": 1651768094,
			"groupIds": [
				"QuJ6k9wmCCsZlezQhd1cM",
				"LsOdPlZlexa5Q8O6BgNyf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974466481,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 586,
			"versionNonce": 22744514,
			"isDeleted": false,
			"id": "pO7EnjUU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -802.9780784255555,
			"y": 660.4635137288966,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 65.8203125,
			"height": 19.2,
			"seed": 1944542082,
			"groupIds": [
				"LsOdPlZlexa5Q8O6BgNyf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974246393,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "configure",
			"rawText": "configure",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "configure"
		},
		{
			"type": "text",
			"version": 685,
			"versionNonce": 536291166,
			"isDeleted": false,
			"id": "ivCyMwH1",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -802.3779250741686,
			"y": 737.2347250482726,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 20.453125,
			"height": 19.2,
			"seed": 55149506,
			"groupIds": [
				"LsOdPlZlexa5Q8O6BgNyf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974246393,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "init",
			"rawText": "init",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "init"
		},
		{
			"type": "text",
			"version": 908,
			"versionNonce": 536163906,
			"isDeleted": false,
			"id": "TXaNMrwG",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -802.7898714156158,
			"y": 891.1434369035592,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 46.25,
			"height": 19.2,
			"seed": 1831505950,
			"groupIds": [
				"LsOdPlZlexa5Q8O6BgNyf"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Th1bxjKVSOLU9iif_hxHV",
					"type": "arrow"
				}
			],
			"updated": 1692974494394,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "assign",
			"rawText": "assign",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "assign"
		},
		{
			"type": "arrow",
			"version": 2125,
			"versionNonce": 536673822,
			"isDeleted": false,
			"id": "CyfSMYJoaTjcf4kFKECwL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -782.2094819623768,
			"y": 748.6107863566046,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 34.25976561656557,
			"height": 0.3312962962703523,
			"seed": 1955079234,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1692973914811,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "IJKn8nVC",
				"focus": 1.8929844956746478,
				"gap": 11.526421315332072
			},
			"endBinding": {
				"elementId": "L9lPbSZ9",
				"focus": -0.0060299102472984815,
				"gap": 2.0561475596555283
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
					34.25976561656557,
					-0.3312962962703523
				]
			]
		},
		{
			"type": "rectangle",
			"version": 1651,
			"versionNonce": 453559554,
			"isDeleted": false,
			"id": "TWxNPeRm4kPAea_sGhvBW",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -847.0112340502789,
			"y": 471.36032913028214,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 273.9821844348747,
			"height": 150.1012263152282,
			"seed": 1241221790,
			"groupIds": [
				"DwYVaYGLFEtRrsh2lEHoQ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692973914812,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 1475,
			"versionNonce": 177439774,
			"isDeleted": false,
			"id": "ajMjK4WzImJF_zekemHTR",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -846.7703323123889,
			"y": 471.5978950967184,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 2.6672282396415166,
			"height": 147.6051056671416,
			"seed": 265740354,
			"groupIds": [
				"DwYVaYGLFEtRrsh2lEHoQ"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1692973914812,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1521,
			"versionNonce": 1030542530,
			"isDeleted": false,
			"id": "TYH4ImLU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -837.8862641975985,
			"y": 474.4994976582088,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 258.59423828125,
			"height": 144,
			"seed": 229410526,
			"groupIds": [
				"DwYVaYGLFEtRrsh2lEHoQ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692973914812,
			"link": null,
			"locked": false,
			"fontSize": 20.144310294391317,
			"fontFamily": 2,
			"text": "struct Config {\n    elem_1: Column<Advice>,\n    elem_2: Column<Advice>,\n    elem_3: Column<Advice>,\n    q_fib: Selector,\n}",
			"rawText": "struct Config {\n    elem_1: Column<Advice>,\n    elem_2: Column<Advice>,\n    elem_3: Column<Advice>,\n    q_fib: Selector,\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "struct Config {\n    elem_1: Column<Advice>,\n    elem_2: Column<Advice>,\n    elem_3: Column<Advice>,\n    q_fib: Selector,\n}"
		},
		{
			"type": "line",
			"version": 3665,
			"versionNonce": 1402004574,
			"isDeleted": false,
			"id": "fqMvNXll2E0-BA5RUDCz-",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0.0023414483691057697,
			"x": -499.2503442760984,
			"y": 474.22012423687653,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 131.58921309211973,
			"height": 201.72578864279456,
			"seed": 84137758,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1692974174548,
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
					-14.423610240098412,
					24.59866910654563
				],
				[
					-16.011622174036972,
					153.84160042080413
				],
				[
					-37.77312002571159,
					194.44871971478193
				],
				[
					-16.26970993223148,
					178.1622578262818
				],
				[
					8.945900244316363,
					199.4631742869234
				],
				[
					93.81609306640814,
					201.72578864279456
				]
			]
		},
		{
			"type": "rectangle",
			"version": 1920,
			"versionNonce": 644294466,
			"isDeleted": false,
			"id": "L8zqLugIvoxjEW2fg7ig0",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -841.2003439603872,
			"y": 994.122253778861,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 271.54650842672885,
			"height": 104.98830302883046,
			"seed": 1075226754,
			"groupIds": [
				"p1LLPde4OFypxjzAoRduG",
				"9cDh5UDMmbwH_0MUajGou"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974665901,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 1754,
			"versionNonce": 879977950,
			"isDeleted": false,
			"id": "B4qA-5oZZfqFKSfWDZQ9o",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -840.9594422224972,
			"y": 994.3598197452973,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.2680645798372785,
			"height": 104.1701120560117,
			"seed": 1885837470,
			"groupIds": [
				"p1LLPde4OFypxjzAoRduG",
				"9cDh5UDMmbwH_0MUajGou"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1692974665901,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1755,
			"versionNonce": 189067010,
			"isDeleted": false,
			"id": "YLtoxDvd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -832.0753741077068,
			"y": 997.2614223067878,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 241.73883056640625,
			"height": 96,
			"seed": 1576159298,
			"groupIds": [
				"p1LLPde4OFypxjzAoRduG",
				"9cDh5UDMmbwH_0MUajGou"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974665901,
			"link": null,
			"locked": false,
			"fontSize": 20.144310294391317,
			"fontFamily": 2,
			"text": "struct MyCircuit<F: Field> {\n    elem_1: Value<F>, // 1\n    elem_2: Value<F>, // 1\n}",
			"rawText": "struct MyCircuit<F: Field> {\n    elem_1: Value<F>, // 1\n    elem_2: Value<F>, // 1\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "struct MyCircuit<F: Field> {\n    elem_1: Value<F>, // 1\n    elem_2: Value<F>, // 1\n}"
		},
		{
			"type": "text",
			"version": 7225,
			"versionNonce": 1795619358,
			"isDeleted": false,
			"id": "VuQEEKsr",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -542.1789956825972,
			"y": 1006.003134687835,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 671.09375,
			"height": 268.8,
			"seed": 212542146,
			"groupIds": [
				"E0wi6YOBoV3I4XwCzJqXP",
				"9cDh5UDMmbwH_0MUajGou"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974665901,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl<F: Field> Circuit<F> for MyCircuit<F> {\n    type Config = Config;\n    type FloorPlanner = SimpleFloorPlanner;\n    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {\n        Self::Config::configure(meta)\n    }\n    fn synthesize(&self, config: Self::Config, mut layouter: impl Layouter<F>) {\n        // return (elem_2 = 1, elem_3 = 2)\n        let (elem_2, elem_3) = config.init(layouter.namespace(|| \"init\"), self.elem_1, self.elem_2)?;\n        // 1 + 2 = 3\n        config.assign(layouter.namespace(|| \"first assign after init\"), elem_2, elem_3)?;\n    }\n\n",
			"rawText": "impl<F: Field> Circuit<F> for MyCircuit<F> {\n    type Config = Config;\n    type FloorPlanner = SimpleFloorPlanner;\n    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {\n        Self::Config::configure(meta)\n    }\n    fn synthesize(&self, config: Self::Config, mut layouter: impl Layouter<F>) {\n        // return (elem_2 = 1, elem_3 = 2)\n        let (elem_2, elem_3) = config.init(layouter.namespace(|| \"init\"), self.elem_1, self.elem_2)?;\n        // 1 + 2 = 3\n        config.assign(layouter.namespace(|| \"first assign after init\"), elem_2, elem_3)?;\n    }\n\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl<F: Field> Circuit<F> for MyCircuit<F> {\n    type Config = Config;\n    type FloorPlanner = SimpleFloorPlanner;\n    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {\n        Self::Config::configure(meta)\n    }\n    fn synthesize(&self, config: Self::Config, mut layouter: impl Layouter<F>) {\n        // return (elem_2 = 1, elem_3 = 2)\n        let (elem_2, elem_3) = config.init(layouter.namespace(|| \"init\"), self.elem_1, self.elem_2)?;\n        // 1 + 2 = 3\n        config.assign(layouter.namespace(|| \"first assign after init\"), elem_2, elem_3)?;\n    }\n\n"
		},
		{
			"type": "rectangle",
			"version": 5189,
			"versionNonce": 1754513090,
			"isDeleted": false,
			"id": "GxiRhkqdAkRfwldT3VGWP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -551.7890329461065,
			"y": 993.1028999830316,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 685.3666925205287,
			"height": 250.32833854665466,
			"seed": 93479134,
			"groupIds": [
				"E0wi6YOBoV3I4XwCzJqXP",
				"9cDh5UDMmbwH_0MUajGou"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974665901,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 494,
			"versionNonce": 1788741214,
			"isDeleted": false,
			"id": "xyfOua3c",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -414.903281165082,
			"y": 1082.9542011782548,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 65.8203125,
			"height": 19.2,
			"seed": 402676162,
			"groupIds": [
				"9cDh5UDMmbwH_0MUajGou"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974665901,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "configure",
			"rawText": "configure",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "configure"
		},
		{
			"type": "text",
			"version": 1544,
			"versionNonce": 1983827550,
			"isDeleted": false,
			"id": "LD2RpKyg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -494.10828424723036,
			"y": 466.0549763079647,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 228.984375,
			"height": 172.79999999999998,
			"seed": 724782530,
			"groupIds": [
				"NwgqDRCJ41TV-9DhmKyYy"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974623645,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "// i = 1, 2, 3 \nlet elem_i = cs.advice_column();\ncs.enable_equality(elem_i);\n\nlet q_fib = cs.selector();\n\n\n\ncs.create_gate",
			"rawText": "// i = 1, 2, 3 \nlet elem_i = cs.advice_column();\ncs.enable_equality(elem_i);\n\nlet q_fib = cs.selector();\n\n\n\ncs.create_gate",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "// i = 1, 2, 3 \nlet elem_i = cs.advice_column();\ncs.enable_equality(elem_i);\n\nlet q_fib = cs.selector();\n\n\n\ncs.create_gate"
		},
		{
			"type": "text",
			"version": 147,
			"versionNonce": 748834434,
			"isDeleted": false,
			"id": "KLpVujyP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -480.71954866877644,
			"y": 465.8910117910772,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 3.5546875,
			"height": 19.2,
			"seed": 1457429890,
			"groupIds": [
				"NwgqDRCJ41TV-9DhmKyYy"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974623645,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "i",
			"rawText": "i",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "i"
		},
		{
			"type": "text",
			"version": 222,
			"versionNonce": 542200478,
			"isDeleted": false,
			"id": "kcyw12We",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -429.0066881272883,
			"y": 485.072500640879,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 3.5546875,
			"height": 19.2,
			"seed": 898177090,
			"groupIds": [
				"NwgqDRCJ41TV-9DhmKyYy"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974623645,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "i",
			"rawText": "i",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "i"
		},
		{
			"type": "text",
			"version": 231,
			"versionNonce": 1176055362,
			"isDeleted": false,
			"id": "2ctpAHhM",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -312.5940999752726,
			"y": 504.3848174000284,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 3.5546875,
			"height": 19.2,
			"seed": 663472386,
			"groupIds": [
				"NwgqDRCJ41TV-9DhmKyYy"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974623645,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "i",
			"rawText": "i",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "i"
		},
		{
			"type": "text",
			"version": 203,
			"versionNonce": 1508761730,
			"isDeleted": false,
			"id": "9Ez8j4kU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -363.85942978403284,
			"y": 650.3885345599035,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 299.71875,
			"height": 19.2,
			"seed": 377196254,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974015293,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "// return q_fib × (ele_1 + elem_2 - elem_3)",
			"rawText": "// return q_fib × (ele_1 + elem_2 - elem_3)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "// return q_fib × (ele_1 + elem_2 - elem_3)"
		},
		{
			"type": "text",
			"version": 1706,
			"versionNonce": 750257822,
			"isDeleted": false,
			"id": "L9lPbSZ9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -745.8935687861557,
			"y": 737.8228747001195,
			"strokeColor": "#028d19",
			"backgroundColor": "transparent",
			"width": 160.1171875,
			"height": 19.2,
			"seed": 2107138014,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "CyfSMYJoaTjcf4kFKECwL",
					"type": "arrow"
				}
			],
			"updated": 1692974323295,
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
			"version": 731,
			"versionNonce": 2040165058,
			"isDeleted": false,
			"id": "IJKn8nVC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -818.8405203584052,
			"y": 717.8843650412725,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 236.4921875,
			"height": 19.2,
			"seed": 689147778,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "CyfSMYJoaTjcf4kFKECwL",
					"type": "arrow"
				}
			],
			"updated": 1692973914813,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "// 初始化 Fibonacci 序列前 2 个数",
			"rawText": "// 初始化 Fibonacci 序列前 2 个数",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "// 初始化 Fibonacci 序列前 2 个数"
		},
		{
			"type": "line",
			"version": 4913,
			"versionNonce": 2096630146,
			"isDeleted": false,
			"id": "LCmUkB00FGsfqRmU3H3FB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0.0023414483691057697,
			"x": -558.9734360869669,
			"y": 692.4439015202237,
			"strokeColor": "#028d19",
			"backgroundColor": "transparent",
			"width": 25.964363879866767,
			"height": 115.36023892389028,
			"seed": 1643971842,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1692974326027,
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
					-10.147375366592428,
					9.854194471531628
				],
				[
					-13.795483056675948,
					49.73155505041666
				],
				[
					-23.80498934824874,
					57.80971766978155
				],
				[
					-13.813549990413094,
					62.30381295074312
				],
				[
					-10.789608048359696,
					105.52328300029717
				],
				[
					2.1593745316180275,
					115.36023892389028
				]
			]
		},
		{
			"type": "text",
			"version": 1899,
			"versionNonce": 229317890,
			"isDeleted": false,
			"id": "LMy2Slkf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -554.6982107089842,
			"y": 681.0147962105807,
			"strokeColor": "#028d19",
			"backgroundColor": "transparent",
			"width": 555.546875,
			"height": 134.4,
			"seed": 633059330,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1692979461122,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let offset = 0;\n// Assign elem_1\nregion.assign_advice(|| \"elem_1\", self.elem_1, offset, || elem_1)?;\nlet elem_2 = region.assign_advice(|| \"elem_2\", self.elem_2, offset, || elem_2)?;\nlet e3_val = elem_1 + elem_2.value_field().evaluate();\nlet elem_3 = region.assign_advice(|| \"elem_3\", self.elem_3, offset, || e3_val)?\nreturn OK((elem_2, elem_3))",
			"rawText": "let offset = 0;\n// Assign elem_1\nregion.assign_advice(|| \"elem_1\", self.elem_1, offset, || elem_1)?;\nlet elem_2 = region.assign_advice(|| \"elem_2\", self.elem_2, offset, || elem_2)?;\nlet e3_val = elem_1 + elem_2.value_field().evaluate();\nlet elem_3 = region.assign_advice(|| \"elem_3\", self.elem_3, offset, || e3_val)?\nreturn OK((elem_2, elem_3))",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let offset = 0;\n// Assign elem_1\nregion.assign_advice(|| \"elem_1\", self.elem_1, offset, || elem_1)?;\nlet elem_2 = region.assign_advice(|| \"elem_2\", self.elem_2, offset, || elem_2)?;\nlet e3_val = elem_1 + elem_2.value_field().evaluate();\nlet elem_3 = region.assign_advice(|| \"elem_3\", self.elem_3, offset, || e3_val)?\nreturn OK((elem_2, elem_3))"
		},
		{
			"type": "arrow",
			"version": 3259,
			"versionNonce": 1454093214,
			"isDeleted": false,
			"id": "Th1bxjKVSOLU9iif_hxHV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -756.1373971257223,
			"y": 903.0468404333806,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 27.648899723982822,
			"height": 0.4976428262096988,
			"seed": 31894430,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974509261,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "TXaNMrwG",
				"focus": 0.2722450794879439,
				"gap": 1
			},
			"endBinding": {
				"elementId": "3rw2zltJ",
				"focus": -0.025570122760409743,
				"gap": 2.35163385168903
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
					27.648899723982822,
					-0.4976428262096988
				]
			]
		},
		{
			"type": "text",
			"version": 2198,
			"versionNonce": 1296432322,
			"isDeleted": false,
			"id": "3rw2zltJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -726.1368635500504,
			"y": 891.1836065347408,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 160.1171875,
			"height": 19.2,
			"seed": 2005189598,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "Th1bxjKVSOLU9iif_hxHV",
					"type": "arrow"
				}
			],
			"updated": 1692974501056,
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
			"version": 1557,
			"versionNonce": 532170370,
			"isDeleted": false,
			"id": "k1rAieSh",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0,
			"x": -364.3236612741683,
			"y": 569.6553680961798,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 453.09375,
			"height": 115.19999999999999,
			"seed": 1505577054,
			"groupIds": [
				"6Qh0jR1BrtC6RnTM2UQLl"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1692974011193,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let q_fib = virtual_cells.query_selector(q_fib);\nlet elem_1 = virtual_cells.query_advice(elem_1, Rotation::cur());\nlet elem_2 = virtual_cells.query_advice(elem_2, Rotation::cur());\nlet elem_3 = virtual_cells.query_advice(elem_3, Rotation::cur());\n\nreturn vec![ q_fib * (ele_1 + elem_2 - elem_3) ]",
			"rawText": "let q_fib = virtual_cells.query_selector(q_fib);\nlet elem_1 = virtual_cells.query_advice(elem_1, Rotation::cur());\nlet elem_2 = virtual_cells.query_advice(elem_2, Rotation::cur());\nlet elem_3 = virtual_cells.query_advice(elem_3, Rotation::cur());\n\nreturn vec![ q_fib * (ele_1 + elem_2 - elem_3) ]",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let q_fib = virtual_cells.query_selector(q_fib);\nlet elem_1 = virtual_cells.query_advice(elem_1, Rotation::cur());\nlet elem_2 = virtual_cells.query_advice(elem_2, Rotation::cur());\nlet elem_3 = virtual_cells.query_advice(elem_3, Rotation::cur());\n\nreturn vec![ q_fib * (ele_1 + elem_2 - elem_3) ]"
		},
		{
			"type": "line",
			"version": 3617,
			"versionNonce": 196532894,
			"isDeleted": false,
			"id": "7jb9LMQxLuohtBzs-ehkq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 40,
			"angle": 0.0023414483691057697,
			"x": -371.0308414322149,
			"y": 579.9922879966233,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 17.582356705933307,
			"height": 103.15543729714557,
			"seed": 1043893954,
			"groupIds": [
				"6Qh0jR1BrtC6RnTM2UQLl"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1692974011193,
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
					-5.632406139651039,
					11.10864669879436
				],
				[
					-5.564246635172481,
					44.516132425983415
				],
				[
					-14.344079374972182,
					52.5498397006636
				],
				[
					-6.403326751175996,
					58.94654685613307
				],
				[
					-3.379256166646826,
					95.25258916361884
				],
				[
					3.2382773309611252,
					103.15543729714557
				]
			]
		},
		{
			"type": "text",
			"version": 253,
			"versionNonce": 58070722,
			"isDeleted": false,
			"id": "YDQePEi5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -549.3415727109657,
			"y": 818.8198983842535,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 751.7734375,
			"height": 172.79999999999998,
			"seed": 1929043202,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1692979358363,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "let offset = 0;\n// Copy elem_1 (which is the previous elem_2)\nlet elem_1 = elem_2.copy_advice(|| \"copy elem_2 into current elem_1\", &mut region, self.elem_1, offset)?;\n// Copy elem_2 (which is the previous elem_3)\nlet elem_2 = elem_3.copy_advice(|| \"copy elem_3 into current elem_2\", &mut region, self.elem_2, offset)?;\nlet e3_val = elem_1.value_field().evaluate() + elem_2.value_field().evaluate();\n// Assign elem_3\nlet elem_3 = region.assign_advice(|| \"elem_3\", self.elem_3, offset, || e3_val)?;\n",
			"rawText": "let offset = 0;\n// Copy elem_1 (which is the previous elem_2)\nlet elem_1 = elem_2.copy_advice(|| \"copy elem_2 into current elem_1\", &mut region, self.elem_1, offset)?;\n// Copy elem_2 (which is the previous elem_3)\nlet elem_2 = elem_3.copy_advice(|| \"copy elem_3 into current elem_2\", &mut region, self.elem_2, offset)?;\nlet e3_val = elem_1.value_field().evaluate() + elem_2.value_field().evaluate();\n// Assign elem_3\nlet elem_3 = region.assign_advice(|| \"elem_3\", self.elem_3, offset, || e3_val)?;\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let offset = 0;\n// Copy elem_1 (which is the previous elem_2)\nlet elem_1 = elem_2.copy_advice(|| \"copy elem_2 into current elem_1\", &mut region, self.elem_1, offset)?;\n// Copy elem_2 (which is the previous elem_3)\nlet elem_2 = elem_3.copy_advice(|| \"copy elem_3 into current elem_2\", &mut region, self.elem_2, offset)?;\nlet e3_val = elem_1.value_field().evaluate() + elem_2.value_field().evaluate();\n// Assign elem_3\nlet elem_3 = region.assign_advice(|| \"elem_3\", self.elem_3, offset, || e3_val)?;\n"
		},
		{
			"type": "line",
			"version": 5261,
			"versionNonce": 667835970,
			"isDeleted": false,
			"id": "eKCiuoSunG8V-H6NpRDeo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0.0023414483691057697,
			"x": -552.4135409992708,
			"y": 848.6790055897548,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 12.174647225510057,
			"height": 115.01157309926555,
			"seed": 1916871326,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1692974554472,
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
					-5.7140267252675585,
					9.843813995615733
				],
				[
					-7.183507527109782,
					49.11129039206958
				],
				[
					-12.073206463196811,
					55.93687096562962
				],
				[
					-7.959015310069276,
					62.83409327799734
				],
				[
					-4.068780562067673,
					105.80888976642257
				],
				[
					0.10144076231324561,
					115.01157309926555
				]
			]
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#5c940d",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 936.9535369941218,
		"scrollY": -426.49053914854693,
		"zoom": {
			"value": 1.1
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