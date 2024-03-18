# kollm_evaluation
ko llm leaderboard와 같은 task 평가를 하기위해 자체 한글 평가 데이터셋을 구축하였고 모델을 평가한 프레임워크입니다.
우리가 만든 nox 모델은 kollm_evaluation을 이용하여 자체 모델을 평가후에 ko llm leaderboard에 upload하여 1위를 차지하였습니다. 

이 저장소는 우리가 한국어 모델을 연구하고 빠르게 모델을 평가할수 있고 어떤 task에서 장/단점이 있는지 확인하기 위한 곳으로 많은 사용자가 이곳을 사용하여 좋은 모델을 개발할수 있도록 도움을 줄수 있을꺼라 믿습니다. 


kollm_evaluation는 [lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) 코드를 사용하고 우리가 구축한 데이터를 추가로 평가할수 있도록 task를 추가하였습니다.

아래는 우리가 구축한 데이터셋으로 영어 데이터셋과 자체 연구한 데이터셋입니다.
- [davidkim205/ko_arc_challenge](https://huggingface.co/datasets/davidkim205/ko_arc_challenge)
- [davidkim205/ko_arc_easy](https://huggingface.co/datasets/davidkim205/ko_arc_easy)
- [davidkim205/ko_truthful_qa](https://huggingface.co/datasets/davidkim205/ko_truthful_qa)
- [davidkim205/ko_hellaswag](https://huggingface.co/datasets/davidkim205/ko_hellaswag)
- [davidkim205/ko_common_gen](https://huggingface.co/datasets/davidkim205/ko_common_gen)

## install
```
conda create -n kollm_evaluation python=3.10
conda activate kollm_evaluation
cd /work/kollm_evaluation

pip install .
pip install -r requirements.txt

```
## lm_eval
```
lm_eval --model hf --model_args pretrained=davidkim205/nox-solar-10.7b-v4,dtype=float16 --num_fewshot 0 --batch_size 16 --tasks ko_hellaswag,kobest_hellaswag,ko_mmlu,ko_arc_easy,ko_truthfulqa,ko_common_gen --device cuda

```

## Result 2024/03/17

| Average     | Ko-TruthfulQA_mc1 | Ko-MMLU | Ko-HellaSwag | Ko-CommonGen V2 | Ko-ARC-Easy | kobest | kobest_boolq | kobest_copa | kobest_hellaswag | kobest_sentineg | kobest_wic |
| ----------- | ----------------- | ------- | ------------ | --------------- | ----------- | ------ | ------------ | ----------- | ---------------- | --------------- | ---------- |
| 69.48       | 63.16             | 48.59   | 85.93        | 87.21           | 71.16       | 60.8   | 55.91        | 77.5        | 57               | 89.67           | 48.81      |
| 66.68       | 55.2              | 46.39   | 84.99        | 85.98           | 68.17       | 59.33  | 50.71        | 75.5        | 59               | 94.46           | 48.81      |
| 62.66       | 49.33             | 38.57   | 80.85        | 83.43           | 65.44       | 58.32  | 50.21        | 71.7        | 58.6             | 95.72           | 48.81      |
| 61.32666667 | 46.88             | 38.59   | 87.07        | 72.08           | 53.07       | 70.27  | 92.59        | 73.7        | 51.6             | 49.37           | 59.37      |
| 60.93       | 45.17             | 37.2    | 86.19        | 78.28           | 49.06       | 69.68  | 90.6         | 73.7        | 54.6             | 57.93           | 56.03      |

https://huggingface.co/spaces/upstage/open-ko-llm-leaderboard

| Model                               | Average | Ko-ARC | Ko-HellaSwag | Ko-MMLU | Ko-TruthfulQA | Ko-CommonGen V2 |
| ----------------------------------- | ------- | ------ | ------------ | ------- | ------------- | --------------- |
| davidkim205/nox-solar-10.7b-v4      | 67.77   | 73.55  | 72.07        | 57.93   | 79.32         | 55.96           |
| davidkim205/nox-solar-10.7b-v2      | 65.38   | 73.46  | 67.32        | 58.7    | 71.94         | 55.49           |
| Deepnoid/deep-solar-v2.0.1          | 61.45   | 72.18  | 57.07        | 54.12   | 66.37         | 57.5            |
| davidkim205/komt-solar-10.7b-sft-v5 | 60.59   | 57.08  | 69.62        | 52.99   | 67.51         | 55.73           |
| Edentns/DataVortexS-10.7B-dpo-v1.11 | 59.556  | 55.97  | 68.68        | 52.67   | 66.74         | 53.72           |

### davidkim205/nox-solar-10.7b-v4
```
lm_eval --model hf --model_args pretrained=davidkim205/nox-solar-10.7b-v4,dtype=bfloat16 --num_fewshot 0 --batch_size 64 --tasks kobest,ko_hellaswag,ko_mmlu,ko_arc_easy,ko_truthfulqa,ko_common_gen --device cuda
```

|       Tasks       |Version|Filter|n-shot| Metric |Value |   |Stderr|
|-------------------|-------|------|-----:|--------|-----:|---|------|
|kobest             |N/A    |none  |     0|acc     |0.6080|±  |0.0069|
|                   |       |none  |     0|f1      |0.5306|±  |N/A   |
| - kobest_boolq    |      1|none  |     0|acc     |0.5591|±  |0.0133|
|                   |       |none  |     0|f1      |0.4536|±  |N/A   |
| - kobest_copa     |      1|none  |     0|acc     |0.7750|±  |0.0132|
|                   |       |none  |     0|f1      |0.7747|±  |N/A   |
| - kobest_hellaswag|      1|none  |     0|acc     |0.4840|±  |0.0224|
|                   |       |none  |     0|f1      |0.4790|±  |N/A   |
|                   |       |none  |     0|acc_norm|0.5700|±  |0.0222|
| - kobest_sentineg |      1|none  |     0|acc     |0.8967|±  |0.0153|
|                   |       |none  |     0|f1      |0.8963|±  |N/A   |
| - kobest_wic      |      1|none  |     0|acc     |0.4881|±  |0.0141|
|                   |       |none  |     0|f1      |0.3280|±  |N/A   |
|ko_truthfulqa      |      2|none  |     0|acc     |0.6316|±  |0.0169|
|ko_mmlu            |      1|none  |     0|acc     |0.4859|±  |0.0027|
|                   |       |none  |     0|acc_norm|0.4859|±  |0.0027|
|ko_hellaswag       |      1|none  |     0|acc     |0.6779|±  |0.0047|
|                   |       |none  |     0|acc_norm|0.8593|±  |0.0035|
|ko_common_gen      |      1|none  |     0|acc     |0.8721|±  |0.0085|
|                   |       |none  |     0|acc_norm|0.8721|±  |0.0085|
|ko_arc_easy        |      1|none  |     0|acc     |0.6655|±  |0.0138|
|                   |       |none  |     0|acc_norm|0.7116|±  |0.0132|

### davidkim205/nox-solar-10.7b-v2
```
lm_eval --model hf --model_args pretrained=davidkim205/nox-solar-10.7b-v2,dtype=float16 --num_fewshot 0 --batch_size 64 --tasks kobest,ko_hellaswag,ko_mmlu,ko_arc_easy,ko_truthfulqa,ko_common_gen --device cuda
```

|       Tasks       |Version|Filter|n-shot| Metric |Value |   |Stderr|
|-------------------|-------|------|-----:|--------|-----:|---|------|
|kobest             |N/A    |none  |     0|acc     |0.5933|±  |0.0069|
|                   |       |none  |     0|f1      |0.4991|±  |N/A   |
| - kobest_boolq    |      1|none  |     0|acc     |0.5071|±  |0.0133|
|                   |       |none  |     0|f1      |0.3465|±  |N/A   |
| - kobest_copa     |      1|none  |     0|acc     |0.7550|±  |0.0136|
|                   |       |none  |     0|f1      |0.7548|±  |N/A   |
| - kobest_hellaswag|      1|none  |     0|acc     |0.4980|±  |0.0224|
|                   |       |none  |     0|f1      |0.4937|±  |N/A   |
|                   |       |none  |     0|acc_norm|0.5900|±  |0.0220|
| - kobest_sentineg |      1|none  |     0|acc     |0.9446|±  |0.0115|
|                   |       |none  |     0|f1      |0.9446|±  |N/A   |
| - kobest_wic      |      1|none  |     0|acc     |0.4881|±  |0.0141|
|                   |       |none  |     0|f1      |0.3280|±  |N/A   |
|ko_truthfulqa      |      2|none  |     0|acc     |0.5520|±  |0.0174|
|ko_mmlu            |      1|none  |     0|acc     |0.4639|±  |0.0027|
|                   |       |none  |     0|acc_norm|0.4639|±  |0.0027|
|ko_hellaswag       |      1|none  |     0|acc     |0.6600|±  |0.0047|
|                   |       |none  |     0|acc_norm|0.8499|±  |0.0036|
|ko_common_gen      |      1|none  |     0|acc     |0.8598|±  |0.0089|
|                   |       |none  |     0|acc_norm|0.8598|±  |0.0089|
|ko_arc_easy        |      1|none  |     0|acc     |0.6391|±  |0.0140|
|                   |       |none  |     0|acc_norm|0.6817|±  |0.0136|


### Deepnoid/deep-solar-v2.0.1
```
lm_eval --model hf --model_args pretrained=Deepnoid/deep-solar-v2.0.1,dtype=float16 --num_fewshot 0 --batch_size 64 --tasks kobest --device cuda
```
|     Tasks      |Version|Filter|n-shot| Metric |Value |   |Stderr|
|----------------|------:|------|-----:|--------|-----:|---|------|
|kobest_hellaswag|      1|none  |     0|acc     |0.4860|±  |0.0224|
|                |       |none  |     0|f1      |0.4827|±  |N/A   |
|                |       |none  |     0|acc_norm|0.5860|±  |0.0220|
|ko_truthfulqa   |      2|none  |     0|acc     |0.4933|±  |0.0175|
|ko_mmlu         |      1|none  |     0|acc     |0.3857|±  |0.0026|
|                |       |none  |     0|acc_norm|0.3857|±  |0.0026|
|ko_hellaswag    |      1|none  |     0|acc     |0.6085|±  |0.0049|
|                |       |none  |     0|acc_norm|0.8085|±  |0.0039|
|ko_common_gen   |      1|none  |     0|acc     |0.8343|±  |0.0095|
|                |       |none  |     0|acc_norm|0.8343|±  |0.0095|
|ko_arc_easy     |      1|none  |     0|acc     |0.6067|±  |0.0143|
|                |       |none  |     0|acc_norm|0.6544|±  |0.0139|

|       Tasks       |Version|Filter|n-shot| Metric |Value |   |Stderr|
|-------------------|-------|------|-----:|--------|-----:|---|------|
|kobest             |N/A    |none  |     0|acc     |0.5832|±  |0.0070|
|                   |       |none  |     0|f1      |0.4869|±  |N/A   |
| - kobest_boolq    |      1|none  |     0|acc     |0.5021|±  |0.0133|
|                   |       |none  |     0|f1      |0.3343|±  |N/A   |
| - kobest_copa     |      1|none  |     0|acc     |0.7170|±  |0.0143|
|                   |       |none  |     0|f1      |0.7166|±  |N/A   |
| - kobest_hellaswag|      1|none  |     0|acc     |0.4860|±  |0.0224|
|                   |       |none  |     0|f1      |0.4827|±  |N/A   |
|                   |       |none  |     0|acc_norm|0.5860|±  |0.0220|
| - kobest_sentineg |      1|none  |     0|acc     |0.9572|±  |0.0102|
|                   |       |none  |     0|f1      |0.9572|±  |N/A   |
| - kobest_wic      |      1|none  |     0|acc     |0.4881|±  |0.0141|
|                   |       |none  |     0|f1      |0.3280|±  |N/A   |

### Edentns/DataVortexS-10.7B-dpo-v1.11
```
lm_eval --model hf --model_args pretrained=Edentns/DataVortexS-10.7B-dpo-v1.11,dtype=float16 --num_fewshot 0 --batch_size 64 --tasks kobest --device cuda

```
hf (pretrained=Edentns/DataVortexS-10.7B-dpo-v1.11,dtype=float16), gen_kwargs: (None), limit: None, num_fewshot: 0, batch_size: 64
|     Tasks      |Version|Filter|n-shot| Metric |Value |   |Stderr|
|----------------|------:|------|-----:|--------|-----:|---|------|
|kobest_hellaswag|      1|none  |     0|acc     |0.4480|±  |0.0223|
|                |       |none  |     0|f1      |0.4461|±  |N/A   |
|                |       |none  |     0|acc_norm|0.5160|±  |0.0224|
|ko_truthfulqa   |      2|none  |     0|acc     |0.4688|±  |0.0175|
|ko_mmlu         |      1|none  |     0|acc     |0.3859|±  |0.0026|
|                |       |none  |     0|acc_norm|0.3859|±  |0.0026|
|ko_hellaswag    |      1|none  |     0|acc     |0.7073|±  |0.0045|
|                |       |none  |     0|acc_norm|0.8707|±  |0.0033|
|ko_common_gen   |      1|none  |     0|acc     |0.7208|±  |0.0115|
|                |       |none  |     0|acc_norm|0.7208|±  |0.0115|
|ko_arc_easy     |      1|none  |     0|acc     |0.4309|±  |0.0145|
|                |       |none  |     0|acc_norm|0.5307|±  |0.0146|

|       Tasks       |Version|Filter|n-shot| Metric |Value |   |Stderr|
|-------------------|-------|------|-----:|--------|-----:|---|------|
|kobest             |N/A    |none  |     0|acc     |0.7027|±  |0.0063|
|                   |       |none  |     0|f1      |0.6823|±  |N/A   |
| - kobest_boolq    |      1|none  |     0|acc     |0.9259|±  |0.0070|
|                   |       |none  |     0|f1      |0.9259|±  |N/A   |
| - kobest_copa     |      1|none  |     0|acc     |0.7370|±  |0.0139|
|                   |       |none  |     0|f1      |0.7367|±  |N/A   |
| - kobest_hellaswag|      1|none  |     0|acc     |0.4480|±  |0.0223|
|                   |       |none  |     0|f1      |0.4461|±  |N/A   |
|                   |       |none  |     0|acc_norm|0.5160|±  |0.0224|
| - kobest_sentineg |      1|none  |     0|acc     |0.4937|±  |0.0251|
|                   |       |none  |     0|f1      |0.3305|±  |N/A   |
| - kobest_wic      |      1|none  |     0|acc     |0.5937|±  |0.0138|
|                   |       |none  |     0|f1      |0.5722|±  |N/A   |

### LDCC/LDCC-SOLAR-10.7B
```
lm_eval --model hf --model_args pretrained=LDCC/LDCC-SOLAR-10.7B,dtype=float16 --num_fewshot 0 --batch_size 64 --tasks kobest --device cuda

```
|     Tasks      |Version|Filter|n-shot| Metric |Value |   |Stderr|
|----------------|------:|------|-----:|--------|-----:|---|------|
|kobest_hellaswag|      1|none  |     0|acc     |0.4660|±  |0.0223|
|                |       |none  |     0|f1      |0.4617|±  |N/A   |
|                |       |none  |     0|acc_norm|0.5460|±  |0.0223|
|ko_truthfulqa   |      2|none  |     0|acc     |0.4517|±  |0.0174|
|ko_mmlu         |      1|none  |     0|acc     |0.3720|±  |0.0026|
|                |       |none  |     0|acc_norm|0.3720|±  |0.0026|
|ko_hellaswag    |      1|none  |     0|acc     |0.6912|±  |0.0046|
|                |       |none  |     0|acc_norm|0.8619|±  |0.0034|
|ko_common_gen   |      1|none  |     0|acc     |0.7828|±  |0.0105|
|                |       |none  |     0|acc_norm|0.7828|±  |0.0105|
|ko_arc_easy     |      1|none  |     0|acc     |0.4147|±  |0.0144|
|                |       |none  |     0|acc_norm|0.4906|±  |0.0146|

|       Tasks       |Version|Filter|n-shot| Metric |Value |   |Stderr|
|-------------------|-------|------|-----:|--------|-----:|---|------|
|kobest             |N/A    |none  |     0|acc     |0.6968|±  |0.0064|
|                   |       |none  |     0|f1      |0.6838|±  |N/A   |
| - kobest_boolq    |      1|none  |     0|acc     |0.9060|±  |0.0078|
|                   |       |none  |     0|f1      |0.9058|±  |N/A   |
| - kobest_copa     |      1|none  |     0|acc     |0.7370|±  |0.0139|
|                   |       |none  |     0|f1      |0.7365|±  |N/A   |
| - kobest_hellaswag|      1|none  |     0|acc     |0.4660|±  |0.0223|
|                   |       |none  |     0|f1      |0.4617|±  |N/A   |
|                   |       |none  |     0|acc_norm|0.5460|±  |0.0223|
| - kobest_sentineg |      1|none  |     0|acc     |0.5793|±  |0.0248|
|                   |       |none  |     0|f1      |0.5196|±  |N/A   |
| - kobest_wic      |      1|none  |     0|acc     |0.5603|±  |0.0140|
|                   |       |none  |     0|f1      |0.5346|±  |N/A   |

----------------------

# Language Model Evaluation Harness

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10256836.svg)](https://doi.org/10.5281/zenodo.10256836)

## Announcement
**A new v0.4.0 release of lm-evaluation-harness is available** !

New updates and features include:

- Internal refactoring
- Config-based task creation and configuration
- Easier import and sharing of externally-defined task config YAMLs
- Support for Jinja2 prompt design, easy modification of prompts + prompt imports from Promptsource
- More advanced configuration options, including output post-processing, answer extraction, and multiple LM generations per document, configurable fewshot settings, and more
- Speedups and new modeling libraries supported, including: faster data-parallel HF model usage, vLLM support, MPS support with HuggingFace, and more
- Logging and usability changes
- New tasks including CoT BIG-Bench-Hard, Belebele, user-defined task groupings, and more

Please see our updated documentation pages in `docs/` for more details.

Development will be continuing on the `main` branch, and we encourage you to give us feedback on what features are desired and how to improve the library further, or ask questions, either in issues or PRs on GitHub, or in the [EleutherAI discord](https://discord.gg/eleutherai)!

## Overview

This project provides a unified framework to test generative language models on a large number of different evaluation tasks.

**Features:**
- Over 60 standard academic benchmarks for LLMs, with hundreds of subtasks and variants implemented.
- Support for models loaded via [transformers](https://github.com/huggingface/transformers/) (including quantization via [AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ)), [GPT-NeoX](https://github.com/EleutherAI/gpt-neox), and [Megatron-DeepSpeed](https://github.com/microsoft/Megatron-DeepSpeed/), with a flexible tokenization-agnostic interface.
- Support for fast and memory-efficient inference with [vLLM](https://github.com/vllm-project/vllm).
- Support for commercial APIs including [OpenAI](https://openai.com), and [TextSynth](https://textsynth.com/).
- Support for evaluation on adapters (e.g. LoRA) supported in [HuggingFace's PEFT library](https://github.com/huggingface/peft).
- Support for local models and benchmarks.
- Evaluation with publicly available prompts ensures reproducibility and comparability between papers.
- Easy support for custom prompts and evaluation metrics.

The Language Model Evaluation Harness is the backend for 🤗 Hugging Face's popular [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard), has been used in [hundreds of papers](https://scholar.google.com/scholar?oi=bibs&hl=en&authuser=2&cites=15052937328817631261,4097184744846514103,1520777361382155671,17476825572045927382,18443729326628441434,14801318227356878622,7890865700763267262,12854182577605049984,15641002901115500560,5104500764547628290), and is used internally by dozens of organizations including NVIDIA, Cohere, BigScience, BigCode, Nous Research, and Mosaic ML.

## Install

To install the `lm-eval` package from the github repository, run:

```bash
git clone https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
pip install -e .
```

We also provide a number of optional dependencies for extended functionality. A detailed table is available at the end of this document.

## Basic Usage

### Hugging Face `transformers`

To evaluate a model hosted on the [HuggingFace Hub](https://huggingface.co/models) (e.g. GPT-J-6B) on `hellaswag` you can use the following command (this assumes you are using a CUDA-compatible GPU):

```bash
lm_eval --model hf \
    --model_args pretrained=EleutherAI/gpt-j-6B \
    --tasks hellaswag \
    --device cuda:0 \
    --batch_size 8
```

Additional arguments can be provided to the model constructor using the `--model_args` flag. Most notably, this supports the common practice of using the `revisions` feature on the Hub to store partially trained checkpoints, or to specify the datatype for running a model:

```bash
lm_eval --model hf \
    --model_args pretrained=EleutherAI/pythia-160m,revision=step100000,dtype="float" \
    --tasks lambada_openai,hellaswag \
    --device cuda:0 \
    --batch_size 8
```

Models that are loaded via both `transformers.AutoModelForCausalLM` (autoregressive, decoder-only GPT style models) and `transformers.AutoModelForSeq2SeqLM` (such as encoder-decoder models like T5) in Huggingface are supported.

Batch size selection can be automated by setting the  ```--batch_size``` flag to ```auto```. This will perform automatic detection of the largest batch size that will fit on your device. On tasks where there is a large difference between the longest and shortest example, it can be helpful to periodically recompute the largest batch size, to gain a further speedup. To do this, append ```:N``` to above flag to automatically recompute the largest batch size ```N``` times. For example, to recompute the batch size 4 times, the command would be:

```bash
lm_eval --model hf \
    --model_args pretrained=EleutherAI/pythia-160m,revision=step100000,dtype="float" \
    --tasks lambada_openai,hellaswag \
    --device cuda:0 \
    --batch_size auto:4
```

The full list of supported arguments are provided [here](./docs/interface.md), and on the terminal by calling `lm_eval -h`. Alternatively, you can use `lm-eval` instead of `lm_eval`.

> [!Note]
> Just like you can provide a local path to `transformers.AutoModel`, you can also provide a local path to `lm_eval` via `--model_args pretrained=/path/to/model`

#### Multi-GPU Evaluation with Hugging Face `accelerate`

We support two main ways of using Hugging Face's [accelerate 🚀](https://github.com/huggingface/accelerate) library for multi-GPU evaluation.

To perform *data-parallel evaluation* (where each GPU loads a **separate full copy** of the model), we leverage the `accelerate` launcher as follows:

```
accelerate launch -m lm_eval --model hf \
    --tasks lambada_openai,arc_easy \
    --batch_size 16
```
(or via `accelerate launch --no-python lm_eval`).

For cases where your model can fit on a single GPU, this allows you to evaluate on K GPUs K times faster than on one.

**WARNING**: This setup does not work with FSDP model sharding, so in `accelerate config` FSDP must be disabled, or the NO_SHARD FSDP option must be used.

The second way of using `accelerate` for multi-GPU evaluation is when your model is *too large to fit on a single GPU.*

In this setting, run the library *outside of the `accelerate` launcher*, but passing `parallelize=True` to `--model_args` as follows:

```
lm_eval --model hf \
    --tasks lambada_openai,arc_easy \
    --model_args parallelize=True \
    --batch_size 16
```

This means that your model's weights will be split across all available GPUs.

For more advanced users or even larger models, we allow for the following arguments when `parallelize=True` as well:
- `device_map_option`: How to split model weights across available GPUs. defaults to "auto".
- `max_memory_per_gpu`: the max GPU memory to use per GPU in loading the model.
- `max_cpu_memory`: the max amount of CPU memory to use when offloading the model weights to RAM.
- `offload_folder`: a folder where model weights will be offloaded to disk if needed.

These two options (`accelerate launch` and `parallelize=True`) are mutually exclusive.

**Note: we do not currently support multi-node evaluations natively, and advise using either an externally hosted server to run inference requests against, or creating a custom integration with your distributed framework [as is done for the GPT-NeoX library](https://github.com/EleutherAI/gpt-neox/blob/main/eval_tasks/eval_adapter.py).**


### Tensor + Data Parallel and Optimized Inference with `vLLM`

We also support vLLM for faster inference on [supported model types](https://docs.vllm.ai/en/latest/models/supported_models.html), especially faster when splitting a model across multiple GPUs. For single-GPU or multi-GPU — tensor parallel, data parallel, or a combination of both — inference, for example:

```bash
lm_eval --model vllm \
    --model_args pretrained={model_name},tensor_parallel_size={GPUs_per_model},dtype=auto,gpu_memory_utilization=0.8,data_parallel_size={model_replicas} \
    --tasks lambada_openai \
    --batch_size auto
```
For a full list of supported vLLM configurations, please reference our vLLM integration and the vLLM documentation.

vLLM occasionally differs in output from Huggingface. We treat Huggingface as the reference implementation, and provide a [script](./scripts/model_comparator.py) for checking the validity of vllm results against HF.

### Model APIs and Inference Servers

Our library also supports the evaluation of models served via several commercial APIs, and we hope to implement support for the most commonly used performant local/self-hosted inference servers.

To call a hosted model, use:

```bash
export OPENAI_API_KEY=YOUR_KEY_HERE
lm_eval --model openai-completions \
    --model_args model=davinci \
    --tasks lambada_openai,hellaswag
```

We also support using your own local inference server with servers that mirror the OpenAI Completions and ChatCompletions APIs.

```bash
lm_eval --model local-chat-completions --tasks gsm8k --model_args model=facebook/opt-125m,base_url=http://{yourip}:8000/v1
```
Note that for externally hosted models, configs such as `--device` and `--batch_size` should not be used and do not function. Just like you can use `--model_args` to pass arbitrary arguments to the model constructor for local models, you can use it to pass arbitrary arguments to the model API for hosted models. See the documentation of the hosting service for information on what arguments they support.

| API or Inference Server                                                                                                   | Implemented?                    | `--model <xxx>` name                                                | Models supported:                                                                             | Request Types:                                             |
|---------------------------------------------------------------------------------------------------------------------------|---------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------|
| OpenAI Completions                                                                                                        | :heavy_check_mark:              | `openai-completions`, `local-completions` | All OpenAI Completions API models                                            | `generate_until`, `loglikelihood`, `loglikelihood_rolling` |
| OpenAI ChatCompletions                                                                                                    | :heavy_check_mark:        | `openai-chat-completions`, `local-chat-completions`                                                               | [All ChatCompletions API models](https://platform.openai.com/docs/guides/gpt)                 | `generate_until` (no logprobs)                             |
| Anthropic                                                                                                                 | :heavy_check_mark:              | `anthropic`                                                         | [Supported Anthropic Engines](https://docs.anthropic.com/claude/reference/selecting-a-model)  | `generate_until` (no logprobs)                             |
| Textsynth                                                                                                                 | :heavy_check_mark:                   | `textsynth`                                                         | [All supported engines](https://textsynth.com/documentation.html#engines)                     | `generate_until`, `loglikelihood`, `loglikelihood_rolling` |
| Cohere                                                                                                                    | [:hourglass: - blocked on Cohere API bug](https://github.com/EleutherAI/lm-evaluation-harness/pull/395) | N/A                                                                 | [All `cohere.generate()` engines](https://docs.cohere.com/docs/models)                        | `generate_until`, `loglikelihood`, `loglikelihood_rolling` |
| [Llama.cpp](https://github.com/ggerganov/llama.cpp) (via [llama-cpp-python](https://github.com/abetlen/llama-cpp-python)) | :heavy_check_mark:              | `gguf`, `ggml`                                                      | [All models supported by llama.cpp](https://github.com/ggerganov/llama.cpp)                   | `generate_until`, `loglikelihood`, (perplexity evaluation not yet implemented) |
| vLLM                                                                                                                      | :heavy_check_mark:       | `vllm`                                                              | [Most HF Causal Language Models](https://docs.vllm.ai/en/latest/models/supported_models.html) | `generate_until`, `loglikelihood`, `loglikelihood_rolling` |
| Mamba                       | :heavy_check_mark:       | `mamba_ssm`                                                                      | [Mamba architecture Language Models via the `mamba_ssm` package](https://huggingface.co/state-spaces) | `generate_until`, `loglikelihood`, `loglikelihood_rolling`                             |
| Huggingface Optimum (Causal LMs)    | ✔️         | `openvino`                                 |     Any decoder-only AutoModelForCausalLM converted with Huggingface Optimum into OpenVINO™ Intermediate Representation (IR) format                           |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`                         | ...                                                      |
| Neuron via AWS Inf2 (Causal LMs)    | ✔️         | `neuronx`                                 |     Any decoder-only AutoModelForCausalLM supported to run on [huggingface-ami image for inferentia2](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)                         |  `generate_until`, `loglikelihood`, `loglikelihood_rolling`                         | ...                                                      |
| Your local inference server!                                                                                              | :heavy_check_mark:                             | `local-completions` or `local-chat-completions` (using `openai-chat-completions` model type)    | Any server address that accepts GET requests using HF models and mirror's OpenAI's Completions or ChatCompletions interface                                  | `generate_until`                                           |                                | ...                |

Models which do not supply logits or logprobs can be used with tasks of type `generate_until` only, while local models, or APIs that supply logprobs/logits of their prompts, can be run on all task types: `generate_until`, `loglikelihood`, `loglikelihood_rolling`, and `multiple_choice`.

For more information on the different task `output_types` and model request types, see [our documentation](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/model_guide.md#interface).

### Other Frameworks

A number of other libraries contain scripts for calling the eval harness through their library. These include [GPT-NeoX](https://github.com/EleutherAI/gpt-neox/blob/main/eval_tasks/eval_adapter.py), [Megatron-DeepSpeed](https://github.com/microsoft/Megatron-DeepSpeed/blob/main/examples/MoE/readme_evalharness.md), and [mesh-transformer-jax](https://github.com/kingoflolz/mesh-transformer-jax/blob/master/eval_harness.py).

To create your own custom integration you can follow instructions from [this tutorial](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/interface.md#external-library-usage).

### Additional Features
> [!Note]
> For tasks unsuitable for direct evaluation — either due risks associated with executing untrusted code or complexities in the evaluation process — the `--predict_only` flag is available to obtain decoded generations for post-hoc evaluation.

If you have a Metal compatible Mac, you can run the eval harness using the MPS back-end by replacing `--device cuda:0` with `--device mps` (requires PyTorch version 2.1 or higher).

> [!Note]
> You can inspect what the LM inputs look like by running the following command:
> ```bash
> python write_out.py \
>     --tasks <task1,task2,...> \
>     --num_fewshot 5 \
>     --num_examples 10 \
>     --output_base_path /path/to/output/folder
> ```
> This will write out one text file for each task.

To verify the data integrity of the tasks you're performing in addition to running the tasks themselves, you can use the `--check_integrity` flag:

```bash
lm_eval --model openai \
    --model_args engine=davinci \
    --tasks lambada_openai,hellaswag \
    --check_integrity
```

## Advanced Usage Tips

For models loaded with the HuggingFace  `transformers` library, any arguments provided via `--model_args` get passed to the relevant constructor directly. This means that anything you can do with `AutoModel` can be done with our library. For example, you can pass a local path via `pretrained=` or use models finetuned with [PEFT](https://github.com/huggingface/peft) by taking the call you would run to evaluate the base model and add `,peft=PATH` to the `model_args` argument:
```bash
lm_eval --model hf \
    --model_args pretrained=EleutherAI/gpt-j-6b,parallelize=True,load_in_4bit=True,peft=nomic-ai/gpt4all-j-lora \
    --tasks openbookqa,arc_easy,winogrande,hellaswag,arc_challenge,piqa,boolq \
    --device cuda:0
```

[GPTQ](https://github.com/PanQiWei/AutoGPTQ) quantized models can be loaded by specifying their file names in `,autogptq=NAME` (or `,autogptq=True` for default names) in the `model_args` argument:

```bash
lm_eval --model hf \
    --model_args pretrained=model-name-or-path,autogptq=model.safetensors,gptq_use_triton=True \
    --tasks hellaswag
```

We support wildcards in task names, for example you can run all of the machine-translated lambada tasks via `--task lambada_openai_mt_*`.

To save evaluation results provide an `--output_path`. We also support logging model responses with the `--log_samples` flag for post-hoc analysis.

Additionally, one can provide a directory with `--use_cache` to cache the results of prior runs. This allows you to avoid repeated execution of the same (model, task) pairs for re-scoring.

For a full list of supported arguments, check out the [interface](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/interface.md) guide in our documentation!

> [!Tip]
> Running lm-evaluation-harness as an external library and can't find (almost) any tasks available? Run `lm_eval.tasks.initialize_tasks()` to load the library's stock tasks before calling `lm_eval.evaluate()` or `lm_eval.simple_evaluate()` !

## Visualizing Results

You can seamlessly visualize and analyze the results of your evaluation harness runs using both Weights & Biases (W&B) and Zeno.

### Zeno

You can use [Zeno](https://zenoml.com) to visualize the results of your eval harness runs.

First, head to [hub.zenoml.com](https://hub.zenoml.com) to create an account and get an API key [on your account page](https://hub.zenoml.com/account).
Add this key as an environment variable:

```bash
export ZENO_API_KEY=[your api key]
```

You'll also need to install the `lm_eval[zeno]` package extra.

To visualize the results, run the eval harness with the `log_samples` and `output_path` flags.
We expect `output_path` to contain multiple folders that represent individual model names.
You can thus run your evaluation on any number of tasks and models and upload all of the results as projects on Zeno.

```bash
lm_eval \
    --model hf \
    --model_args pretrained=EleutherAI/gpt-j-6B \
    --tasks hellaswag \
    --device cuda:0 \
    --batch_size 8 \
    --log_samples \
    --output_path output/gpt-j-6B
```

Then, you can upload the resulting data using the `zeno_visualize` script:

```bash
python scripts/zeno_visualize.py \
    --data_path output \
    --project_name "Eleuther Project"
```

This will use all subfolders in `data_path` as different models and upload all tasks within these model folders to Zeno.
If you run the eval harness on multiple tasks, the `project_name` will be used as a prefix and one project will be created per task.

You can find an example of this workflow in [examples/visualize-zeno.ipynb](examples/visualize-zeno.ipynb).

### Weights and Biases

With the [Weights and Biases](https://wandb.ai/site) integration, you can now spend more time extracting deeper insights into your evaluation results. The integration is designed to streamline the process of logging and visualizing experiment results using the Weights & Biases (W&B) platform.

The integration provide functionalities

- to automatically log the evaluation results,
- log the samples as W&B Tables for easy visualization,
- log the `results.json` file as an artifact for version control,
- log the `<task_name>_eval_samples.json` file if the samples are logged,
- generate a comprehensive report for analysis and visualization with all the important metric,
- log task and cli specific configs,
- and more out of the box like the command used to run the evaluation, GPU/CPU counts, timestamp, etc.

First you'll need to install the lm_eval[wandb] package extra. Do `pip install lm_eval[wandb]`.

Authenticate your machine with an your unique W&B token. Visit https://wandb.ai/authorize to get one. Do `wandb login` in your command line terminal.

Run eval harness as usual with a `wandb_args` flag. Use this flag to provide arguments for initializing a wandb run ([wandb.init](https://docs.wandb.ai/ref/python/init)) as comma separated string arguments.

```bash
lm_eval \
    --model hf \
    --model_args pretrained=microsoft/phi-2,trust_remote_code=True \
    --tasks hellaswag,mmlu_abstract_algebra \
    --device cuda:0 \
    --batch_size 8 \
    --output_path output/phi-2 \
    --limit 10 \
    --wandb_args project=lm-eval-harness-integration \
    --log_samples
```

In the stdout, you will find the link to the W&B run page as well as link to the generated report. You can find an example of this workflow in [examples/visualize-wandb.ipynb](examples/visualize-wandb.ipynb).

## How to Contribute or Learn More?

For more information on the library and how everything fits together, check out all of our [documentation pages](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/docs)! We plan to post a larger roadmap of desired + planned library improvements soon, with more information on how contributors can help.

### Implementing new tasks

To implement a new task in the eval harness, see [this guide](./docs/new_task_guide.md).

In general, we follow this priority list for addressing concerns about prompting and other eval details:
1. If there is widespread agreement among people who train LLMs, use the agreed upon procedure.
2. If there is a clear and unambiguous official implementation, use that procedure.
3. If there is widespread agreement among people who evaluate LLMs, use the agreed upon procedure.
4. If there are multiple common implementations but not universal or widespread agreement, use our preferred option among the common implementations. As before, prioritize choosing from among the implementations found in LLM training papers.

These are guidelines and not rules, and can be overruled in special circumstances.

We try to prioritize agreement with the procedures used by other groups to decrease the harm when people inevitably compare runs across different papers despite our discouragement of the practice. Historically, we also prioritized the implementation from [Language Models are Few Shot Learners](https://arxiv.org/abs/2005.14165) as our original goal was specifically to compare results with that paper.

### Support

The best way to get support is to open an issue on this repo or join the [EleutherAI Discord server](https://discord.gg/eleutherai). The `#lm-thunderdome` channel is dedicated to developing this project and the `#release-discussion` channel is for receiving support for our releases. If you've used the library and have had a positive (or negative) experience, we'd love to hear from you!

## Optional Extras
Extras dependencies can be installed via `pip install -e ".[NAME]"`

| Name          | Use                                   |
|---------------|---------------------------------------|
| anthropic     | For using Anthropic's models          |
| dev           | For linting PRs and contributions     |
| gptq          | For loading models with GPTQ          |
| hf_transfer   | For speeding up HF Hub file downloads |
| ifeval        | For running the IFEval task           |
| neuronx       | For running on AWS inf2 instances     |
| mamba         | For loading Mamba SSM models          |
| math          | For running math task answer checking |
| multilingual  | For multilingual tokenizers           |
| openai        | For using OpenAI's models             |
| optimum       | For running Intel OpenVINO models     |
| promptsource  | For using PromptSource prompts        |
| sentencepiece | For using the sentencepiece tokenizer |
| testing       | For running library test suite        |
| vllm          | For loading models with vLLM          |
| zeno          | For visualizing results with Zeno     |
|---------------|---------------------------------------|
| all           | Loads all extras (not recommended)    |

## Cite as

```
@misc{eval-harness,
  author       = {Gao, Leo and Tow, Jonathan and Abbasi, Baber and Biderman, Stella and Black, Sid and DiPofi, Anthony and Foster, Charles and Golding, Laurence and Hsu, Jeffrey and Le Noac'h, Alain and Li, Haonan and McDonell, Kyle and Muennighoff, Niklas and Ociepa, Chris and Phang, Jason and Reynolds, Laria and Schoelkopf, Hailey and Skowron, Aviya and Sutawika, Lintang and Tang, Eric and Thite, Anish and Wang, Ben and Wang, Kevin and Zou, Andy},
  title        = {A framework for few-shot language model evaluation},
  month        = 12,
  year         = 2023,
  publisher    = {Zenodo},
  version      = {v0.4.0},
  doi          = {10.5281/zenodo.10256836},
  url          = {https://zenodo.org/records/10256836}
}
```
