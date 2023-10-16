---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
proof_generate ^GU2F9Ysk

let empty_circuit = empty_circuit();

let (pk, _vk) = generate_keys(&params, &empty_circuit);



let hammster_circuit = create_circuit(a_vec, b_vec);

let proof = generate_proof(&params, &pk, hammster_circuit, &hamming_dist); ^eNPBVyrc

generate_keys {
  plonk::keygen_vk(params, circuit);
  plonk::keygen_pk(params, vk, circuit); } ^ijm9HkvX

// Run setup
const params = hm.setup_params(6);   //..
const proof = hm.proof_generate(input0arr, input1arr, params); ^D9iKh2ZK

wasm.rs ^L7sdNGdI

ProveForm.tsx ^gjsciMQa

hammster ^yTQRaj78

generate_proof() {
  let mut transcript = Blake2bWrite::<_, _, Challenge255<_>>::init(vec![]);
  create_proof(params, pk, &[circuit], &[&[pub_input]], OsRng, &mut transcript)
    .expect("Prover should not fail.");
  transcript.finalize() } ^wvy1Jlej

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
			"version": 269,
			"versionNonce": 312596748,
			"isDeleted": false,
			"id": "GU2F9Ysk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -288.3497099426834,
			"y": -196.19648716985566,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 165.14390563964844,
			"height": 33.6,
			"seed": 561081320,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786653,
			"link": null,
			"locked": false,
			"fontSize": 28,
			"fontFamily": 4,
			"text": "proof_generate",
			"rawText": "proof_generate",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "proof_generate"
		},
		{
			"type": "text",
			"version": 116,
			"versionNonce": 1137511860,
			"isDeleted": false,
			"id": "eNPBVyrc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -105.84255393136709,
			"y": -234.12573446719787,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 621.9993896484375,
			"height": 216,
			"seed": 1321650328,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786654,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "let empty_circuit = empty_circuit();\n\nlet (pk, _vk) = generate_keys(&params, &empty_circuit);\n\n\n\nlet hammster_circuit = create_circuit(a_vec, b_vec);\n\nlet proof = generate_proof(&params, &pk, hammster_circuit, &hamming_dist);",
			"rawText": "let empty_circuit = empty_circuit();\n\nlet (pk, _vk) = generate_keys(&params, &empty_circuit);\n\n\n\nlet hammster_circuit = create_circuit(a_vec, b_vec);\n\nlet proof = generate_proof(&params, &pk, hammster_circuit, &hamming_dist);",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "let empty_circuit = empty_circuit();\n\nlet (pk, _vk) = generate_keys(&params, &empty_circuit);\n\n\n\nlet hammster_circuit = create_circuit(a_vec, b_vec);\n\nlet proof = generate_proof(&params, &pk, hammster_circuit, &hamming_dist);"
		},
		{
			"type": "text",
			"version": 811,
			"versionNonce": 1973081996,
			"isDeleted": false,
			"id": "ijm9HkvX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 23.87280105996723,
			"y": -161.22344041809714,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 263.51971435546875,
			"height": 57.599999999999994,
			"seed": 130634136,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786655,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "generate_keys {\n  plonk::keygen_vk(params, circuit);\n  plonk::keygen_pk(params, vk, circuit); }",
			"rawText": "generate_keys {\n  plonk::keygen_vk(params, circuit);\n  plonk::keygen_pk(params, vk, circuit); }",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "generate_keys {\n  plonk::keygen_vk(params, circuit);\n  plonk::keygen_pk(params, vk, circuit); }"
		},
		{
			"type": "text",
			"version": 349,
			"versionNonce": 1741240116,
			"isDeleted": false,
			"id": "D9iKh2ZK",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -293.4680915306055,
			"y": -340.0767824858329,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 399.8395690917969,
			"height": 57.599999999999994,
			"seed": 523848424,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786655,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "// Run setup\nconst params = hm.setup_params(6);   //..\nconst proof = hm.proof_generate(input0arr, input1arr, params);",
			"rawText": "// Run setup\nconst params = hm.setup_params(6);   //..\nconst proof = hm.proof_generate(input0arr, input1arr, params);",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "// Run setup\nconst params = hm.setup_params(6);   //..\nconst proof = hm.proof_generate(input0arr, input1arr, params);"
		},
		{
			"type": "line",
			"version": 251,
			"versionNonce": 1795558552,
			"isDeleted": false,
			"id": "_uhLXpPWtfXEjOMdC1P-4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -183.45495458022845,
			"y": -276.6156060373614,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 95.80771447960677,
			"height": 1.397114486886096,
			"seed": 505158552,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694088081067,
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
					95.80771447960677,
					-1.397114486886096
				]
			]
		},
		{
			"type": "text",
			"version": 177,
			"versionNonce": 1039140364,
			"isDeleted": false,
			"id": "L7sdNGdI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -289.5240702363298,
			"y": -155.7770863079472,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 46.63993835449219,
			"height": 19.2,
			"seed": 1141380072,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786655,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "wasm.rs",
			"rawText": "wasm.rs",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "wasm.rs"
		},
		{
			"type": "text",
			"version": 214,
			"versionNonce": 1568944308,
			"isDeleted": false,
			"id": "gjsciMQa",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -295.9941115021534,
			"y": -260.4767222885922,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 86.7999267578125,
			"height": 19.2,
			"seed": 1292574440,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786655,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "ProveForm.tsx",
			"rawText": "ProveForm.tsx",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "ProveForm.tsx"
		},
		{
			"type": "text",
			"version": 132,
			"versionNonce": 1352361100,
			"isDeleted": false,
			"id": "yTQRaj78",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 403.1092726271622,
			"y": -151.5571372877946,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 57.80793762207031,
			"height": 19.2,
			"seed": 1349915544,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786655,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "hammster",
			"rawText": "hammster",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "hammster"
		},
		{
			"type": "text",
			"version": 797,
			"versionNonce": 614726196,
			"isDeleted": false,
			"id": "wvy1Jlej",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -4.72151345706817,
			"y": -15.720886323364084,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 528.5274658203125,
			"height": 96,
			"seed": 1084785048,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1694182786656,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "generate_proof() {\n  let mut transcript = Blake2bWrite::<_, _, Challenge255<_>>::init(vec![]);\n  create_proof(params, pk, &[circuit], &[&[pub_input]], OsRng, &mut transcript)\n    .expect(\"Prover should not fail.\");\n  transcript.finalize() }",
			"rawText": "generate_proof() {\n  let mut transcript = Blake2bWrite::<_, _, Challenge255<_>>::init(vec![]);\n  create_proof(params, pk, &[circuit], &[&[pub_input]], OsRng, &mut transcript)\n    .expect(\"Prover should not fail.\");\n  transcript.finalize() }",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "generate_proof() {\n  let mut transcript = Blake2bWrite::<_, _, Challenge255<_>>::init(vec![]);\n  create_proof(params, pk, &[circuit], &[&[pub_input]], OsRng, &mut transcript)\n    .expect(\"Prover should not fail.\");\n  transcript.finalize() }"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#000000",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 4,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 318.11693001789655,
		"scrollY": 395.0503495561136,
		"zoom": {
			"value": 1.6500000000000001
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