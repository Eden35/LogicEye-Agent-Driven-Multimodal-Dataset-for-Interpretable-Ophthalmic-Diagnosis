#LogicEye: Agent-Driven Multimodal Dataset for Interpretable Ophthalmic Diagnosis
LogicEye is a framework for building and evaluating interpretable multimodal ophthalmic models. It combines an agent-based data annotation pipeline, a large-scale fundus/OCT image–report dataset, and a hierarchical vision-language benchmark. The goal is to move beyond coarse disease labels toward visually grounded diagnostic descriptions that include both supporting findings and clinically relevant absent signs.

Project status: This README is based on the accompanying paper. Public repository URLs, released checkpoints, dataset download instructions, and software installation details were not specified in the paper and should be added when available.

Highlights
Large-scale multimodal data: 804,461 ophthalmic images collected from 67 public sources, including color fundus photographs and optical coherence tomography (OCT) images.
Fine-grained clinical descriptions: Image–report pairs describe pathological findings and diagnostic evidence rather than relying only on coarse labels.
Positive and negative evidence: LogicEyeAgent retrieves disease inclusion and exclusion criteria, verifies candidate features visually, and integrates present as well as absent findings into the report.
Dual quality verification: Generated annotations are assessed by a multimodal large language model (MLLM), with controversial samples referred for clinical expert review.
Hierarchical evaluation and training: LogicEyeBench combines report generation with coarse-grained disease classification and fine-grained subtype recognition.
Project Components
Component	Description
LogicEyeAgent	A dual-pool inference and verification agent for generating visually grounded diagnostic reports. It uses positive feature criteria for inclusion and negative feature criteria for exclusion.
LogicEyeData	A multimodal ophthalmic dataset containing fundus and OCT images with detailed pathological diagnostic reports.
LogicEyeScore	An annotation assessment score that considers diagnostic accuracy, feature completeness, and hallucination/non-hallucination. Samples below the paper's stated review threshold are flagged for manual review.
LogicEyeBench	A vision-language instruction-tuning and evaluation framework with text generation and hierarchical disease classification objectives.
Dataset at a Glance
The paper reports that LogicEyeData aggregates data from 67 public sources.

Item	Reported value
Total images	804,461
Color fundus photographs	346,649
OCT images	457,812
Major disease categories used for coarse-grained classification	9
Fine-grained subtypes used for classification	29
Ophthalmic diseases represented in the structured expert knowledge base	38
Fine-grained pathological features noted in the dataset comparison	304
Reported pixel-level annotation data points	Approximately 120 million
The paper describes a hierarchy of major disease categories and fine-grained subtypes, along with visually grounded pathological features. The 38 diseases in the expert knowledge base, 9 coarse-grained categories, 29 classification subtypes, and 304 fine-grained features refer to different levels or elements of the framework and are therefore reported separately here.

Modalities
Color fundus photographs (CFP): Used to describe retinal and optic-disc findings.
Optical coherence tomography (OCT): Used to describe retinal-layer and macular findings.
The dataset was assembled from public sources. Before redistributing or reusing any source data, users should check the original dataset's license, access conditions, and permitted use. The paper does not specify a single license for the combined LogicEyeData collection.

Method Overview
1. LogicEyeAgent: evidence-grounded report generation
For a given disease label, the agent retrieves two complementary sets of diagnostic criteria:

Positive feature pool: Findings expected to support a diagnosis, such as microaneurysms or hard exudates.
Negative feature pool: Findings whose absence helps with exclusion or differential diagnosis, such as the absence of neovascularization in applicable disease stages.
The agent then verifies candidate features against the image. Visible findings are described with their anatomical locations; absent findings are represented as negative evidence. The final report combines these facts into a structured natural-language description.

2. Annotation quality control
The paper describes MLLM-based evaluation across three dimensions:

Diagnostic accuracy: Whether the broad disease category and fine-grained subtype are correctly identified.
Feature completeness: Whether key pathological findings are included.
Non-hallucination: Whether the report avoids describing findings that are not present in the image.
The three scores are combined into LogicEyeScore. Samples below the paper's stated threshold are flagged for manual review; controversial cases are sent to senior ophthalmologists for final assessment.

3. LogicEyeBench: joint text generation and classification
LogicEyeBench is initialized from the Qwen3-VL-8B architecture. It jointly optimizes:

Autoregressive diagnostic report generation (L_gen)
Coarse-grained disease classification (L_coarse)
Fine-grained subtype classification (L_fine)
The total objective is reported as:

[
L_{total} = L_{gen} + \lambda_1 L_{coarse} + \lambda_2 L_{fine}
]

The two classification objectives are intended to keep generated descriptions aligned with disease categories and subtypes, reducing semantic drift between visual findings and diagnostic predictions.

Reported Experimental Results
The following values are transcribed from the paper's reported test-set comparison; they have not been independently reproduced as part of this README.

Comparison with general-purpose MLLMs
Model	Parameters	LogicEyeScore	ACC	METEOR	ROUGE-L
LogicEyeBench	8B	2.92	0.54	0.47	0.44
GPT-4o	Proprietary	2.11	0.32	0.28	0.27
Gemini 3.0 Flash	Proprietary	2.05	0.28	0.26	0.25
Qwen3-VL-Plus	Proprietary	1.80	0.25	0.24	0.23
Ablation of dual-granularity loss weights
(\lambda_1)	(\lambda_2)	LogicEyeScore	ACC	METEOR	ROUGE-L
0	0	2.31	0.41	0.46	0.43
1	0	2.68	0.49	0.48	0.45
0	1	2.61	0.47	0.47	0.44
1	1	2.92	0.54	0.47	0.44
1	2	2.89	0.53	0.45	0.42
2	2	2.83	0.51	0.43	0.40
According to the paper, setting both loss weights to 1 achieved the best reported LogicEyeScore and classification accuracy in this ablation.

Training Details Reported in the Paper
Setting	Value
Initialization	Qwen3-VL-8B
Optimizer	AdamW
Initial learning rate	1e-5
Learning-rate schedule	Cosine annealing
Batch size	4
Training epochs	10
GPUs	8 × NVIDIA A100
Train/validation/test split	8:1:1
Visual encoder	Frozen during the initial training stages
Trainable components described	Cross-modal adapter and LLM parameters
These details summarize the paper and are not a complete, executable training recipe. Exact preprocessing, dependency versions, data manifests, hyperparameter configuration files, and launch commands were not provided in the supplied paper.

Installation
Installation instructions will be added when the project code and its dependency configuration are released.

BASH
复制
# Coming soon: clone the repository and install project dependencies.
Usage
Inference, dataset loading, training, and evaluation commands are not included in the supplied paper. Usage examples will be added when the corresponding code and model checkpoints are available.

Data Access and Responsible Use
The paper describes a dataset aggregated from 67 public ophthalmic data sources, but does not provide a consolidated download URL or a project-level license.
Users should comply with the terms and licenses of each original dataset and verify whether redistribution and downstream use are permitted.
Ophthalmic images and diagnostic reports may be sensitive medical data. Apply appropriate privacy, security, and institutional review requirements when working with clinical data.
This research framework is intended for research and evaluation. It is not a standalone medical device and should not be used as a substitute for professional clinical judgment.
Citation
If you use this work, please cite the paper. The supplied manuscript lists the title below but has anonymized authors and does not identify a final publication venue or DOI; replace this placeholder with the official citation when available.

TEXT
复制
LogicEye: Agent Driven Multimodal Dataset for Interpretable Ophthalmic Diagnosis.
Authors anonymized in the supplied manuscript.
Acknowledgements
LogicEyeData was constructed from public ophthalmic datasets. Please consult the original sources and cite them as required when using their data.

Contact
Project contact details will be added when the authors' public project page or contact address is available.
