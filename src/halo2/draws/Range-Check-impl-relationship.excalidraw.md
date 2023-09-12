---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
pub trait FloorPlanner {
    fn synthesize<F: Field, CS: Assignment<F>, C: Circuit<F>>(
        cs: &mut CS,
        circuit: &C,
        config: C::Config,
        constants: Vec<Column<Fixed>>,
    ) -> Result<(), Error>;
} ^kRucXMcC

synthesize ^j8Y23xtm

FloorPlanner ^e3ruWqIz

only definition, no implement ^rTxxureq

pub trait Circuit<F: Field> {
    type Config: Clone;
    type FloorPlanner: FloorPlanner;
    fn without_witnesses(&self) -> Self;
    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config;
    fn synthesize(&self, config: Self::Config,
          layouter: impl Layouter<F>) -> Result<(), Error>;
 ^FemI9krr

Circuit ^vcvg2ybZ

synthesize ^Kgm3odL5

struct MyCircuit<F: FieldExt, const RANGE: usize,
  const LOOKUP_RANGE: usize> {
    simple_value: Value<Assigned<F>>,
    lookup_value: Value<Assigned<F>>,
} ^RpDkL3RH

impl<F: FieldExt, const R: usize, const L..R: usize> Circuit<F>
    for MyCircuit<F, R, L..R>
{
    type Config = RangeCheckConfig<F, RANGE, L..R>;
    type FloorPlanner = V1;
    fn synthesize(
 ^tooRU01U

FloorPlanner ^G8LZ4ceA

#[test]
let circuit = MyCircuit::<Fp, RANGE, LOOKUP_RANGE> {
    simple_value: Value::known(Fp::from(6 as u64).into()),
    lookup_value: Value::known(Fp::from(200 as u64).into()),
};

let prover = MockProver::run(k, &circuit, vec![]).unwrap();
 ^E0Tv3cA0

run ^DEe0yZol

impl<F: FieldExt> MockProver<F> {
    pub fn run<ConcreteCircuit: Circuit<F>>(
        k: u32,
        circuit: &ConcreteCircuit,
        instance: Vec<Vec<F>>,
    )
    /// ...
    ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config..)?;
    /// ...
} ^y1TbePmk

(RANGE) ^M8aiaxaB

(LOOKUP_RANGE) ^wJSS5aoW

associate type ^SknHiCqH

instantiate
(实例化) ^ZjVS3zOW

Circuit ^VtZfdDSt

implement ^0b4qQQQH

F
F
F
F
 ^vrbEvL5G

PASTING EXCALIDRAW ELEMENTS AS A TEXT ELEMENT IS NOT ALLOWED ^cVpW3U4k

MyCircuit ^21k8Zecs

run ^jwbpvIfe

ConcreteCircuit::V1::synthesize ... ^569OhEE7

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
			"version": 6654,
			"versionNonce": 301243110,
			"isDeleted": false,
			"id": "kRucXMcC",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -438.2273868384981,
			"y": 474.77071934808265,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 443.71875,
			"height": 153.6,
			"seed": 659487206,
			"groupIds": [
				"PElWfpmdlLcqI-JoQ94JB",
				"L2qt4JfQYZ4NsCwZ5Q2dB"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "pub trait FloorPlanner {\n    fn synthesize<F: Field, CS: Assignment<F>, C: Circuit<F>>(\n        cs: &mut CS,\n        circuit: &C,\n        config: C::Config,\n        constants: Vec<Column<Fixed>>,\n    ) -> Result<(), Error>;\n}",
			"rawText": "pub trait FloorPlanner {\n    fn synthesize<F: Field, CS: Assignment<F>, C: Circuit<F>>(\n        cs: &mut CS,\n        circuit: &C,\n        config: C::Config,\n        constants: Vec<Column<Fixed>>,\n    ) -> Result<(), Error>;\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "pub trait FloorPlanner {\n    fn synthesize<F: Field, CS: Assignment<F>, C: Circuit<F>>(\n        cs: &mut CS,\n        circuit: &C,\n        config: C::Config,\n        constants: Vec<Column<Fixed>>,\n    ) -> Result<(), Error>;\n}"
		},
		{
			"type": "rectangle",
			"version": 4958,
			"versionNonce": 694382202,
			"isDeleted": false,
			"id": "NGssnjCW0TmrBhaENAlla",
			"fillStyle": "hachure",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -445.91279745105055,
			"y": 462.635668708678,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 465.31085576512106,
			"height": 166.27204929239292,
			"seed": 1956806522,
			"groupIds": [
				"PElWfpmdlLcqI-JoQ94JB",
				"L2qt4JfQYZ4NsCwZ5Q2dB"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 527,
			"versionNonce": 598494758,
			"isDeleted": false,
			"id": "j8Y23xtm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -402.54169119531446,
			"y": 493.84358934723605,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 75.59375,
			"height": 19.2,
			"seed": 1249546342,
			"groupIds": [
				"L2qt4JfQYZ4NsCwZ5Q2dB"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "synthesize",
			"rawText": "synthesize",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "synthesize"
		},
		{
			"type": "text",
			"version": 659,
			"versionNonce": 1990430522,
			"isDeleted": false,
			"id": "e3ruWqIz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -375.9117734263109,
			"y": 474.80033656129785,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 91.6015625,
			"height": 19.2,
			"seed": 1433286694,
			"groupIds": [
				"L2qt4JfQYZ4NsCwZ5Q2dB"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "FloorPlanner",
			"rawText": "FloorPlanner",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FloorPlanner"
		},
		{
			"type": "text",
			"version": 513,
			"versionNonce": 1653315942,
			"isDeleted": false,
			"id": "rTxxureq",
			"fillStyle": "hachure",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0.22441220528639416,
			"x": -201.8696938934621,
			"y": 552.2881203721018,
			"strokeColor": "#91cee8",
			"backgroundColor": "transparent",
			"width": 202.78125,
			"height": 19.2,
			"seed": 1695490618,
			"groupIds": [
				"L2qt4JfQYZ4NsCwZ5Q2dB"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "only definition, no implement",
			"rawText": "only definition, no implement",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "only definition, no implement"
		},
		{
			"type": "rectangle",
			"version": 1044,
			"versionNonce": 279025658,
			"isDeleted": false,
			"id": "daiT3UxmFAb4wcCUmKzNw",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -988.4093610216419,
			"y": 681.978763075289,
			"strokeColor": "#000000",
			"backgroundColor": "#868e96",
			"width": 366.08752427107845,
			"height": 105.98449201587616,
			"seed": 1699454458,
			"groupIds": [
				"vqkOuaqy6mVv2cYJjDLs7"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 741,
			"versionNonce": 1977580710,
			"isDeleted": false,
			"id": "VqhNl-TcLqPqeHvQy0TCE",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 50,
			"angle": 0,
			"x": -988.1684592837519,
			"y": 682.2163290417252,
			"strokeColor": "#000000",
			"backgroundColor": "#000000",
			"width": 4.001759464155249,
			"height": 107.12985259755796,
			"seed": 490788518,
			"groupIds": [
				"vqkOuaqy6mVv2cYJjDLs7"
			],
			"roundness": {
				"type": 1
			},
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 552,
			"versionNonce": 2002815162,
			"isDeleted": false,
			"id": "RpDkL3RH",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -978.6500494217986,
			"y": 686.6094030954908,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 355.1953125,
			"height": 96,
			"seed": 1960431290,
			"groupIds": [
				"vqkOuaqy6mVv2cYJjDLs7"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "struct MyCircuit<F: FieldExt, const RANGE: usize,\n  const LOOKUP_RANGE: usize> {\n    simple_value: Value<Assigned<F>>,\n    lookup_value: Value<Assigned<F>>,\n}",
			"rawText": "struct MyCircuit<F: FieldExt, const RANGE: usize,\n  const LOOKUP_RANGE: usize> {\n    simple_value: Value<Assigned<F>>,\n    lookup_value: Value<Assigned<F>>,\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "struct MyCircuit<F: FieldExt, const RANGE: usize,\n  const LOOKUP_RANGE: usize> {\n    simple_value: Value<Assigned<F>>,\n    lookup_value: Value<Assigned<F>>,\n}"
		},
		{
			"type": "text",
			"version": 6677,
			"versionNonce": 324335590,
			"isDeleted": false,
			"id": "tooRU01U",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -980.0611337450939,
			"y": 813.2468743596376,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 439.265625,
			"height": 134.4,
			"seed": 854551846,
			"groupIds": [
				"UYymRozIeMI4Qsh0YZH3g"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl<F: FieldExt, const R: usize, const L..R: usize> Circuit<F>\n    for MyCircuit<F, R, L..R>\n{\n    type Config = RangeCheckConfig<F, RANGE, L..R>;\n    type FloorPlanner = V1;\n    fn synthesize(\n",
			"rawText": "impl<F: FieldExt, const R: usize, const L..R: usize> Circuit<F>\n    for MyCircuit<F, R, L..R>\n{\n    type Config = RangeCheckConfig<F, RANGE, L..R>;\n    type FloorPlanner = V1;\n    fn synthesize(\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl<F: FieldExt, const R: usize, const L..R: usize> Circuit<F>\n    for MyCircuit<F, R, L..R>\n{\n    type Config = RangeCheckConfig<F, RANGE, L..R>;\n    type FloorPlanner = V1;\n    fn synthesize(\n"
		},
		{
			"type": "rectangle",
			"version": 4981,
			"versionNonce": 847470970,
			"isDeleted": false,
			"id": "XBpFWIi1fZiVpDPgQ9ooJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -986.9824963675374,
			"y": 801.0934604899619,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 495.44360137708526,
			"height": 136.04371509622655,
			"seed": 1206247482,
			"groupIds": [
				"UYymRozIeMI4Qsh0YZH3g"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "U0pxx2zL9AoewZFTjloJx",
					"type": "arrow"
				}
			],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 6672,
			"versionNonce": 1565336122,
			"isDeleted": false,
			"id": "FemI9krr",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -965.1961041938507,
			"y": 475.5407483157302,
			"strokeColor": "#495057",
			"backgroundColor": "#fff",
			"width": 463.7421875,
			"height": 153.6,
			"seed": 43602662,
			"groupIds": [
				"FPnI7b5mEdlVsehNYTm-_",
				"eK_JuM8J1WKPe87a_NG6Y"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "pub trait Circuit<F: Field> {\n    type Config: Clone;\n    type FloorPlanner: FloorPlanner;\n    fn without_witnesses(&self) -> Self;\n    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config;\n    fn synthesize(&self, config: Self::Config,\n          layouter: impl Layouter<F>) -> Result<(), Error>;\n",
			"rawText": "pub trait Circuit<F: Field> {\n    type Config: Clone;\n    type FloorPlanner: FloorPlanner;\n    fn without_witnesses(&self) -> Self;\n    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config;\n    fn synthesize(&self, config: Self::Config,\n          layouter: impl Layouter<F>) -> Result<(), Error>;\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "pub trait Circuit<F: Field> {\n    type Config: Clone;\n    type FloorPlanner: FloorPlanner;\n    fn without_witnesses(&self) -> Self;\n    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config;\n    fn synthesize(&self, config: Self::Config,\n          layouter: impl Layouter<F>) -> Result<(), Error>;\n"
		},
		{
			"type": "rectangle",
			"version": 4992,
			"versionNonce": 1127790182,
			"isDeleted": false,
			"id": "GbwBQ7h4sumdJCJ6JvYG_",
			"fillStyle": "hachure",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -972.8814520108563,
			"y": 463.40569767632553,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 477.46819925667097,
			"height": 157.63653125915482,
			"seed": 210122362,
			"groupIds": [
				"FPnI7b5mEdlVsehNYTm-_",
				"eK_JuM8J1WKPe87a_NG6Y"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "U0pxx2zL9AoewZFTjloJx",
					"type": "arrow"
				}
			],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 760,
			"versionNonce": 1558247846,
			"isDeleted": false,
			"id": "vcvg2ybZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -902.834154317136,
			"y": 475.8020183166184,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 45.3359375,
			"height": 19.2,
			"seed": 1567460794,
			"groupIds": [
				"XO83FFRfDMlQyBWXIC7Su",
				"eK_JuM8J1WKPe87a_NG6Y"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Circuit",
			"rawText": "Circuit",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Circuit"
		},
		{
			"type": "text",
			"version": 623,
			"versionNonce": 419519418,
			"isDeleted": false,
			"id": "Kgm3odL5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -929.5766491136203,
			"y": 571.5019418993622,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 75.59375,
			"height": 19.2,
			"seed": 1531351162,
			"groupIds": [
				"_UI-z77ayKPh8Rnj2tEYD",
				"eK_JuM8J1WKPe87a_NG6Y"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "synthesize",
			"rawText": "synthesize",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "synthesize"
		},
		{
			"type": "text",
			"version": 748,
			"versionNonce": 1998509286,
			"isDeleted": false,
			"id": "G8LZ4ceA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -912.6619441376655,
			"y": 513.688526866689,
			"strokeColor": "#2b8a3e",
			"backgroundColor": "transparent",
			"width": 91.6015625,
			"height": 19.2,
			"seed": 504273062,
			"groupIds": [
				"gQ18uvyh4opo-EYm3QAQS",
				"eK_JuM8J1WKPe87a_NG6Y"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "FloorPlanner",
			"rawText": "FloorPlanner",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FloorPlanner"
		},
		{
			"type": "text",
			"version": 532,
			"versionNonce": 781309050,
			"isDeleted": false,
			"id": "E0Tv3cA0",
			"fillStyle": "hachure",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -440.8180604052741,
			"y": 653.5308604846524,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 421.25,
			"height": 153.6,
			"seed": 1697956262,
			"groupIds": [
				"bvnL9Su7Wc11VEfYjbpOY"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "MbAoxoYrbYE4HlWnSpxFE",
					"type": "arrow"
				}
			],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "#[test]\nlet circuit = MyCircuit::<Fp, RANGE, LOOKUP_RANGE> {\n    simple_value: Value::known(Fp::from(6 as u64).into()),\n    lookup_value: Value::known(Fp::from(200 as u64).into()),\n};\n\nlet prover = MockProver::run(k, &circuit, vec![]).unwrap();\n",
			"rawText": "#[test]\nlet circuit = MyCircuit::<Fp, RANGE, LOOKUP_RANGE> {\n    simple_value: Value::known(Fp::from(6 as u64).into()),\n    lookup_value: Value::known(Fp::from(200 as u64).into()),\n};\n\nlet prover = MockProver::run(k, &circuit, vec![]).unwrap();\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "#[test]\nlet circuit = MyCircuit::<Fp, RANGE, LOOKUP_RANGE> {\n    simple_value: Value::known(Fp::from(6 as u64).into()),\n    lookup_value: Value::known(Fp::from(200 as u64).into()),\n};\n\nlet prover = MockProver::run(k, &circuit, vec![]).unwrap();\n"
		},
		{
			"type": "text",
			"version": 542,
			"versionNonce": 1427453242,
			"isDeleted": false,
			"id": "DEe0yZol",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -262.0743963297805,
			"y": 768.8601919605833,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 23.125,
			"height": 19.2,
			"seed": 1148154278,
			"groupIds": [
				"bvnL9Su7Wc11VEfYjbpOY"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "run",
			"rawText": "run",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "run"
		},
		{
			"type": "text",
			"version": 6806,
			"versionNonce": 435114854,
			"isDeleted": false,
			"id": "M8aiaxaB",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -839.8354212466458,
			"y": 789.7531375951605,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "#fff",
			"width": 67.5546875,
			"height": 19.2,
			"seed": 1672028794,
			"groupIds": [
				"6x0T4g0a_kcCHU83UzzQn"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "(RANGE)",
			"rawText": "(RANGE)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "(RANGE)"
		},
		{
			"type": "text",
			"version": 6911,
			"versionNonce": 1039039994,
			"isDeleted": false,
			"id": "wJSS5aoW",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 80,
			"angle": 0,
			"x": -754.3149562981665,
			"y": 790.4393181178456,
			"strokeColor": "#c9d9dd",
			"backgroundColor": "#fff",
			"width": 143.140625,
			"height": 19.2,
			"seed": 862458554,
			"groupIds": [
				"H61fh62mt3qcfbGLXHnvm"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "(LOOKUP_RANGE)",
			"rawText": "(LOOKUP_RANGE)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "(LOOKUP_RANGE)"
		},
		{
			"type": "freedraw",
			"version": 103,
			"versionNonce": 1018951334,
			"isDeleted": false,
			"id": "Mi3i5oOpgRBOgkrLL4iV-",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -806.9060140683715,
			"y": 810.2123886966832,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 0,
			"height": 9.867077646293753,
			"seed": 366213926,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					5.894356316935455
				],
				[
					0,
					6.045500311737785
				],
				[
					0,
					5.974588458061391
				],
				[
					0,
					4.8142903396893235
				],
				[
					0,
					-2.077518545611382
				],
				[
					0,
					-2.811724189427565
				],
				[
					0,
					-3.1750600618250076
				],
				[
					0,
					-3.447989145964577
				],
				[
					0,
					-3.8215773345559683
				],
				[
					0,
					5.894356316935455
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 205,
			"versionNonce": 1981853370,
			"isDeleted": false,
			"id": "ylITrTFOIyFYwAfW0CrdZ",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -692.8899000614196,
			"y": 810.7962168441725,
			"strokeColor": "#d0d9dd",
			"backgroundColor": "transparent",
			"width": 6.785131078078614,
			"height": 14.59970651334596,
			"seed": 1520625318,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"points": [
				[
					0.00012708280246600092,
					9.727985120119126
				],
				[
					-0.000030549645373412204,
					9.561726680997682
				],
				[
					-0.000030549645373412204,
					8.911158141132187
				],
				[
					-0.000030549645373412204,
					7.812717198083013
				],
				[
					0.47160573382291565,
					5.317741987216258
				],
				[
					1.5435063780691605,
					3.948032456547825
				],
				[
					2.476059938563511,
					2.5124054830557414
				],
				[
					4.214430571614566,
					0.5856020085253442
				],
				[
					4.214430571614566,
					0.44397105845879853
				],
				[
					4.214430571614566,
					0.26755034690240026
				],
				[
					4.488080500792773,
					0.23285213741604788
				],
				[
					5.476751212662227,
					-0.6254478993473254
				],
				[
					6.099714645906472,
					-1.1808023551345912
				],
				[
					6.78510052843324,
					-1.935373971454959
				],
				[
					6.78510052843324,
					-2.382513958607224
				],
				[
					6.78510052843324,
					-2.7315101975556
				],
				[
					6.78510052843324,
					-2.9376853103868066
				],
				[
					6.78510052843324,
					-3.320921998801985
				],
				[
					6.451550269135303,
					-4.257315894910184
				],
				[
					6.118630539628431,
					-4.871721393226835
				],
				[
					6.118630539628431,
					-4.871721393226835
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "arrow",
			"version": 631,
			"versionNonce": 2089955814,
			"isDeleted": false,
			"id": "kR3ntTmV58WsZgz44DXHK",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -862.115248531994,
			"y": 517.0897795944787,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 520.994565522731,
			"height": 62.89196885077581,
			"seed": 141108646,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "SknHiCqH"
				}
			],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					136.01706254400028,
					-62.89196885077581
				],
				[
					404.1844332710631,
					-61.55792434706535
				],
				[
					520.994565522731,
					-39.33834783293025
				]
			]
		},
		{
			"type": "text",
			"version": 79,
			"versionNonce": 1917765498,
			"isDeleted": false,
			"id": "SknHiCqH",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -655.1460488830033,
			"y": 440.3469351650221,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 131.25,
			"height": 19.2,
			"seed": 331678246,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "associate type",
			"rawText": "associate type",
			"textAlign": "right",
			"verticalAlign": "middle",
			"containerId": "kR3ntTmV58WsZgz44DXHK",
			"originalText": "associate type"
		},
		{
			"type": "rectangle",
			"version": 224,
			"versionNonce": 880739622,
			"isDeleted": false,
			"id": "FUXUNiGt0P5bQ3ZVsoNQd",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -914.8591601863159,
			"y": 514.97125797845,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 96.2943428935306,
			"height": 19.876309597192687,
			"seed": 1934049702,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 307,
			"versionNonce": 301311930,
			"isDeleted": false,
			"id": "MbAoxoYrbYE4HlWnSpxFE",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -887.9780927843324,
			"y": 687.1973331669122,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 445.18741861889055,
			"height": 28.571448447941066,
			"seed": 1039315366,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "ZjVS3zOW"
				}
			],
			"updated": 1693400094421,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "E0Tv3cA0",
				"focus": -0.1810142497335914,
				"gap": 1.9726137601677465
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					99.7860349057255,
					-28.571448447941066
				],
				[
					375.7460009831492,
					-25.083864951311966
				],
				[
					445.18741861889055,
					-1.7047014788259958
				]
			]
		},
		{
			"type": "text",
			"version": 63,
			"versionNonce": 1441755238,
			"isDeleted": false,
			"id": "ZjVS3zOW",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -715.0431920095143,
			"y": 636.8684693465398,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 103.125,
			"height": 38.4,
			"seed": 56394022,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "instantiate\n(实例化)",
			"rawText": "instantiate\n(实例化)",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "MbAoxoYrbYE4HlWnSpxFE",
			"originalText": "instantiate\n(实例化)"
		},
		{
			"type": "rectangle",
			"version": 178,
			"versionNonce": 1464512762,
			"isDeleted": false,
			"id": "IPfzkB35YvHYIq0mZmWsW",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -421.25640042256566,
			"y": 675.1040244675447,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 137.91940024559813,
			"height": 18.48024550735306,
			"seed": 1466944122,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 869,
			"versionNonce": 481325990,
			"isDeleted": false,
			"id": "VtZfdDSt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -615.2898705489575,
			"y": 813.2622954754453,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 45.3359375,
			"height": 19.2,
			"seed": 714615270,
			"groupIds": [
				"iBxVisix0-BCWO6_S7Dpx",
				"oapE7opTXiqE3GM1HT-3G"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Circuit",
			"rawText": "Circuit",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Circuit"
		},
		{
			"type": "arrow",
			"version": 625,
			"versionNonce": 454860922,
			"isDeleted": false,
			"id": "U0pxx2zL9AoewZFTjloJx",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -982.5711661327554,
			"y": 505.94746505151966,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 23.844538114690295,
			"height": 365.8125639830526,
			"seed": 107383930,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "vrbEvL5G"
				}
			],
			"updated": 1693400094421,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "GbwBQ7h4sumdJCJ6JvYG_",
				"focus": 0.9648192891078669,
				"gap": 9.689714121899073
			},
			"endBinding": {
				"elementId": "XBpFWIi1fZiVpDPgQ9ooJ",
				"focus": -0.9261514125750253,
				"gap": 1.7666034665312793
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-20.657369873455423,
					45.41660809747128
				],
				[
					-23.844538114690295,
					312.6610094729428
				],
				[
					-6.177933701313236,
					365.8125639830526
				]
			]
		},
		{
			"type": "text",
			"version": 126,
			"versionNonce": 1122963174,
			"isDeleted": false,
			"id": "vrbEvL5G",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1029.1873288378529,
			"y": 633.6776138566962,
			"strokeColor": "#ffffff",
			"backgroundColor": "transparent",
			"width": 9.375,
			"height": 96,
			"seed": 1318999738,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "F\nF\nF\nF\n",
			"rawText": "F\nF\nF\nF\n",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "U0pxx2zL9AoewZFTjloJx",
			"originalText": "F\nF\nF\nF\n"
		},
		{
			"type": "text",
			"version": 415,
			"versionNonce": 1920612986,
			"isDeleted": false,
			"id": "0b4qQQQH",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 1.5734140732090927,
			"x": -1048.0890098796108,
			"y": 677.8538118321904,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 84.375,
			"height": 19.2,
			"seed": 1534500390,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "implement",
			"rawText": "implement",
			"textAlign": "right",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "implement"
		},
		{
			"type": "text",
			"version": 14,
			"versionNonce": 329758438,
			"isDeleted": false,
			"id": "cVpW3U4k",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1196.8201157906515,
			"y": 974.0721038612937,
			"strokeColor": "#ffffff",
			"backgroundColor": "transparent",
			"width": 562.5,
			"height": 19.2,
			"seed": 725617786,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693399847000,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "PASTING EXCALIDRAW ELEMENTS AS A TEXT ELEMENT IS NOT ALLOWED",
			"rawText": "PASTING EXCALIDRAW ELEMENTS AS A TEXT ELEMENT IS NOT ALLOWED",
			"textAlign": "right",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "PASTING EXCALIDRAW ELEMENTS AS A TEXT ELEMENT IS NOT ALLOWED"
		},
		{
			"type": "text",
			"version": 934,
			"versionNonce": 892879398,
			"isDeleted": false,
			"id": "21k8Zecs",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -939.5576500385622,
			"y": 832.4971847303411,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 66.6640625,
			"height": 19.2,
			"seed": 510572026,
			"groupIds": [
				"4_oj37VIoYHesJilxRW91",
				"poJNfkyFoVwLDajDE0rNI"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "MyCircuit",
			"rawText": "MyCircuit",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "MyCircuit"
		},
		{
			"type": "rectangle",
			"version": 85,
			"versionNonce": 184605498,
			"isDeleted": false,
			"id": "lBbSkf25u8RQeHI6Iglme",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -454.7229392767499,
			"y": 643.2315364026504,
			"strokeColor": "#ffffff",
			"backgroundColor": "transparent",
			"width": 375.2727006706733,
			"height": 151.53403376516872,
			"seed": 33194234,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 145,
			"versionNonce": 1182338406,
			"isDeleted": false,
			"id": "AJkeSrwQRL7ekqzqE0ynP",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -453.6092639874039,
			"y": 649.745788611683,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 454.6174346631707,
			"height": 145.79317319816298,
			"seed": 224274726,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 641,
			"versionNonce": 1131766778,
			"isDeleted": false,
			"id": "y1TbePmk",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -446.4233514678936,
			"y": 821.9855640076436,
			"strokeColor": "#7bcbdb",
			"backgroundColor": "transparent",
			"width": 534.4296875,
			"height": 192,
			"seed": 503198714,
			"groupIds": [
				"gY5Di7pWvxYi6xajehk0c",
				"QiLw8zdgj73fYuPXHlVJv"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "impl<F: FieldExt> MockProver<F> {\n    pub fn run<ConcreteCircuit: Circuit<F>>(\n        k: u32,\n        circuit: &ConcreteCircuit,\n        instance: Vec<Vec<F>>,\n    )\n    /// ...\n    ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config..)?;\n    /// ...\n}",
			"rawText": "impl<F: FieldExt> MockProver<F> {\n    pub fn run<ConcreteCircuit: Circuit<F>>(\n        k: u32,\n        circuit: &ConcreteCircuit,\n        instance: Vec<Vec<F>>,\n    )\n    /// ...\n    ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config..)?;\n    /// ...\n}",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "impl<F: FieldExt> MockProver<F> {\n    pub fn run<ConcreteCircuit: Circuit<F>>(\n        k: u32,\n        circuit: &ConcreteCircuit,\n        instance: Vec<Vec<F>>,\n    )\n    /// ...\n    ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config..)?;\n    /// ...\n}"
		},
		{
			"type": "rectangle",
			"version": 300,
			"versionNonce": 1677619366,
			"isDeleted": false,
			"id": "q-5TWAhJmBEbyHdZl-CtY",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -456.44651919979015,
			"y": 821.7955620845273,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 549.2522472085416,
			"height": 198.82250833705598,
			"seed": 1309137722,
			"groupIds": [
				"QiLw8zdgj73fYuPXHlVJv"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 632,
			"versionNonce": 903533754,
			"isDeleted": false,
			"id": "jwbpvIfe",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -379.7642113752595,
			"y": 841.120350114373,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 23.125,
			"height": 19.2,
			"seed": 636264762,
			"groupIds": [
				"tzGEO2s2w6eZX9XVmrW_d"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "run",
			"rawText": "run",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "run"
		},
		{
			"type": "line",
			"version": 258,
			"versionNonce": 1232432102,
			"isDeleted": false,
			"id": "rnUPttwcY2OnBvIiBcKIu",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -250.37054209648022,
			"y": 790.0414464480853,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 115.20950774971061,
			"height": 53.88551602096345,
			"seed": 570132026,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1693400094312,
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
					-16.923412205268846,
					16.992240000304378
				],
				[
					-92.60285838857294,
					21.412879441496216
				],
				[
					-115.20950774971061,
					53.88551602096345
				]
			]
		},
		{
			"type": "line",
			"version": 124,
			"versionNonce": 1137978746,
			"isDeleted": false,
			"id": "ENk3msuyec9pF9Vds2oeD",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -929.1152482230647,
			"y": 911.9560147092782,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 138.13327920034942,
			"height": 0.8618761048052193,
			"seed": 1904968166,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1693400094312,
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
					138.13327920034942,
					-0.8618761048052193
				]
			]
		},
		{
			"type": "arrow",
			"version": 691,
			"versionNonce": 718748710,
			"isDeleted": false,
			"id": "lEvpdVYiIRjyYh1qr31VN",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -820.1890444262938,
			"y": 910.1899689277111,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 455.3751947243703,
			"height": 83.66054675248256,
			"seed": 1377055846,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1693400094422,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "569OhEE7",
				"focus": -0.5603115354672612,
				"gap": 9.936983828897667
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					212.5456206562435,
					80.86574583814081
				],
				[
					455.3751947243703,
					83.66054675248256
				]
			]
		},
		{
			"type": "text",
			"version": 935,
			"versionNonce": 581201466,
			"isDeleted": false,
			"id": "569OhEE7",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -354.8768658730258,
			"y": 979.5956448061829,
			"strokeColor": "#100eb4",
			"backgroundColor": "transparent",
			"width": 240.984375,
			"height": 19.2,
			"seed": 1094285414,
			"groupIds": [
				"3kQiy0Mmjbx87oLaHuxSz",
				"HeldTiEbYX-cBOS9LtrLz"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "lEvpdVYiIRjyYh1qr31VN",
					"type": "arrow"
				}
			],
			"updated": 1693400094312,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "ConcreteCircuit::V1::synthesize ...",
			"rawText": "ConcreteCircuit::V1::synthesize ...",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "ConcreteCircuit::V1::synthesize ..."
		},
		{
			"type": "rectangle",
			"version": 166,
			"versionNonce": 1731106554,
			"isDeleted": false,
			"id": "D3_JYzROUTCXJ7ZU-IzRV",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -311.4370529538987,
			"y": 957.2867830280348,
			"strokeColor": "#91cee8",
			"backgroundColor": "transparent",
			"width": 95.2720701358279,
			"height": 18.309964298497107,
			"seed": 1336976250,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 340,
			"versionNonce": 1508240806,
			"isDeleted": false,
			"id": "Ka8TiefUK49C6-EtGWPpn",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -235.49823584089978,
			"y": 980.2563631019774,
			"strokeColor": "#91cee8",
			"backgroundColor": "transparent",
			"width": 18.176078254810193,
			"height": 19.72824746230309,
			"seed": 1562294118,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1693400094312,
			"link": null,
			"locked": false
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#7bcbdb",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 0.5,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "right",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 1238.5100538239856,
		"scrollY": -263.767705546279,
		"zoom": {
			"value": 1.05
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