# D6 Command Verification Tracker

## Scope

This page tracks reported execution of installation and quick-start commands for [D6 in the Cookbook Refactor Roadmap](https://github.com/sgl-project/sglang-omni/issues/2839).
It follows the D0 documentation rules in that issue and the [documentation workflow](../README.md).

A result applies only to the recorded command scope, revision, configuration, and hardware.
A tested revision does not certify all earlier revisions or the current branch.
A documentation rebase, PR approval, or merge does not extend runtime verification to a newer commit.
Installation and serving are separate scopes, as are CUDA, native MLX, and Python Torch/MPS.

## Recorded results

Initial evidence review: 2026-10-25.
These rows summarize the linked contributors' reports; creating this tracker did not rerun their commands.
Follow each evidence link for the exact commands, environment, and limitations.
PR links identify evidence and proposed fixes; their merge state is independent of the reported result.

| Documentation path and command scope | Execution path and hardware | Tested source revision | Reported result and limits | Evidence and reporter |
| --- | --- | --- | --- | --- |
| [Drama 3](../cookbook/fishaudio_drama3.md), FP8 checkpoint startup. | CUDA, source install inside `hongccc/sglang-omni:dev`, one A100-SXM4-80GB. | [`1240a6993`](https://github.com/sgl-project/sglang-omni/commit/1240a6993c8073593aa0ad074859ccee2797adff). | Failed during first prefill graph capture: `fp8e4nv` unsupported on sm80 with the reported stack. | [#2997](https://github.com/sgl-project/sglang-omni/pull/2997), [yaourt137](https://github.com/yaourt137). |
| [Drama 3](../cookbook/fishaudio_drama3.md#int4-checkpoint), INT4 startup and both plain TTS and voice-cloning `curl` examples. | CUDA, same container/source path and A100 as above. | [`1240a6993`](https://github.com/sgl-project/sglang-omni/commit/1240a6993c8073593aa0ad074859ccee2797adff). | Passed with adjustments: cold startup needed `SGLANG_OMNI_STARTUP_TIMEOUT=1800`; warm startup passed at the default timeout. Both requests returned HTTP 200 and decodable 44.1 kHz WAVs. No listening or WER assessment. | [#2997](https://github.com/sgl-project/sglang-omni/pull/2997), [yaourt137](https://github.com/yaourt137). |
| [Qwen3-TTS](../cookbook/qwen3_tts.md), basic non-streaming requests for Base 0.6B/1.7B, CustomVoice 0.6B/1.7B, and VoiceDesign 1.7B. | CUDA, configured source environment, one RTX 4090 24 GiB. | [`f073b1c`](https://github.com/sgl-project/sglang-omni/commit/f073b1c6e3e6a5143f16ced1b9bd5d0c24ef9d76). | Passed with adjustments: running/graph limits 8/8 after default predictor capture failed. Valid WAVs and manual listening review. Installation and the whole Cookbook are outside this row's scope. | [#3106](https://github.com/sgl-project/sglang-omni/pull/3106), [henani1110](https://github.com/henani1110). |
| [Shared TTS guide](../basic_usage/tts.md), Qwen3-TTS 0.6B Base reference-audio URL and domain allowlist. | CUDA, configured source environment, same RTX 4090 and 8/8 limits. | [`f073b1c`](https://github.com/sgl-project/sglang-omni/commit/f073b1c6e3e6a5143f16ced1b9bd5d0c24ef9d76). | Passed with adjustment: adding `us.aws.cdn.hf.co` changed HTTP 400 to 200 with a valid, manually reviewed WAV; an unrelated domain still returned 400. Redirect hosts can vary. | [#3106](https://github.com/sgl-project/sglang-omni/pull/3106), [henani1110](https://github.com/henani1110). |
| [Qwen3-TTS](../cookbook/qwen3_tts.md) startup and [shared TTS guide](../basic_usage/tts.md) reference-audio request, 0.6B Base only. | CUDA, RTX 4090 as reported by the contributor. | [`f073b1c`](https://github.com/sgl-project/sglang-omni/commit/f073b1c6e3e6a5143f16ced1b9bd5d0c24ef9d76). | Independent reproduction: default capture failed, 8/8 allowed startup, and adding the CDN domain changed 400 to 200. Valid WAV and matching automatic transcription; no manual listening, newer PR-head test, or 16/17 boundary retest. | [Reproduction on #3106](https://github.com/sgl-project/sglang-omni/pull/3106#issuecomment-6385705040), [fengxiaohu](https://github.com/fengxiaohu). |

**Runtime details.**

- #2997 reports SGLang 0.5.21 and Torch 2.13.0 with CUDA 13.0; its container tag is mutable and no image digest is provided.
- #3106 reports Python 3.12.14, Torch 2.14.1+cu130, Transformers 5.19.0, SGLang 0.5.22, and driver 580.178.04 for the author's runs.
- The independent reproduction on #3106 reports its own environment in the linked comment; it is a separate run.
- The historical `d07df0b` capacity experiment in #3106 is separate from its `f073b1c` command verification and does not certify a maximum for other models or revisions.

## Coverage still to record

The following is an initial collection checklist based on the [documentation tree at `85d309f`](https://github.com/sgl-project/sglang-omni/tree/85d309fbca53d651272df9a55600bdf5c8867d19/docs).
It is not an exhaustive command inventory.
“Unrecorded” means this tracker has no scoped execution report for that entry; it does not mean nobody has tested it.
Contributors can add existing public evidence without rerunning a command, provided its revision and scope are explicit.
Partial results above leave the rest of the corresponding page unrecorded.

| Documentation path | Execution scope to record | Coverage |
| --- | --- | --- |
| [CUDA installation](../get_started/installation.md#option-a-docker-recommended). | Docker pull, container launch, and installation inside the container; record the image digest and effective installed revision/version separately. | Unrecorded. |
| [CUDA installation](../get_started/installation.md#option-b-manual-install). | Manual package installation and prerequisite checks. | Unrecorded. |
| [CUDA installation](../get_started/installation.md#install-from-source). | Clean source installation, including model extras where required. | Unrecorded; serving in a configured environment does not cover it. |
| [MLX path](../mlx/index.md) and [Voxt backend guide](https://github.com/sgl-project/sglang-omni/blob/main/Voxt/backend/README.md). | Native MLX build/install and model server commands, split by model and entry point. | Unrecorded. |
| [Apple Silicon installation](../get_started/installation_apple_silicon.md), [Qwen3-ASR](../cookbook/qwen3_asr.md#apple-silicon), and [Fun-ASR](../cookbook/fun_asr.md#apple-silicon). | Python Torch/MPS installation and serving; record separately from native MLX. | Unrecorded. |
| [Qwen3-TTS](../cookbook/qwen3_tts.md) and [shared TTS guide](../basic_usage/tts.md). | Remaining install, streaming, deployment, and tuning commands outside the recorded scopes. | Partial; see the recorded results. |
| [Drama 3](../cookbook/fishaudio_drama3.md). | Remaining install and deployment commands outside the recorded scopes. | Partial; see the recorded results. |
| [`docs/cookbook/arkasr.md`](../cookbook/arkasr.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/auk.md`](../cookbook/auk.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/cosmos3_nano.md`](../cookbook/cosmos3_nano.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/dots_tts.md`](../cookbook/dots_tts.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/fishaudio_s2_pro.md`](../cookbook/fishaudio_s2_pro.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/fun_asr.md`](../cookbook/fun_asr.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/fun_cosyvoice3.md`](../cookbook/fun_cosyvoice3.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/higgs_tts.md`](../cookbook/higgs_tts.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/llada2_uni.md`](../cookbook/llada2_uni.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/ming3_asr.md`](../cookbook/ming3_asr.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/ming3_audio.md`](../cookbook/ming3_audio.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/ming3_tts.md`](../cookbook/ming3_tts.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/ming_omni.md`](../cookbook/ming_omni.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/ming_tts.md`](../cookbook/ming_tts.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/minicpm_o.md`](../cookbook/minicpm_o.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/minimax_music3.md`](../cookbook/minimax_music3.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/moss_transcribe_diarize.md`](../cookbook/moss_transcribe_diarize.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/moss_tts.md`](../cookbook/moss_tts.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/moss_tts_local.md`](../cookbook/moss_tts_local.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/nemotron3_5_asr.md`](../cookbook/nemotron3_5_asr.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/nemotron_diarization.md`](../cookbook/nemotron_diarization.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/nemotron_voicechat.md`](../cookbook/nemotron_voicechat.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/personaplex.md`](../cookbook/personaplex.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/qwen3_asr.md`](../cookbook/qwen3_asr.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/qwen3_omni.md`](../cookbook/qwen3_omni.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/voxtral_tts.md`](../cookbook/voxtral_tts.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/whisper_asr.md`](../cookbook/whisper_asr.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [`docs/cookbook/zonos2.md`](../cookbook/zonos2.md). | CUDA install and quick-start commands; split model variants and execution paths when adding results. | Unrecorded. |
| [API server quick start](../get_started/apiserver_quickstart.md), other [general usage guides](../index.rst), and [repository README](https://github.com/sgl-project/sglang-omni/blob/main/README.md). | Additional CUDA/MLX install or quick-start snippets and links to their canonical command records. | Unrecorded. |
| [Ascend NPU installation](../get_started/installation_npu.md) and Ascend-specific Cookbook commands. | Huawei/Ascend local command execution. | Exempt from local verification under D6; not reported as passed. |

## Add or update a result

1. Identify the documentation file and section or snippet, backend, installation method, model/checkpoint revision, and configuration.
2. Record the actual test date and exact source commit or package version; for containers, include the image digest and any code installed or updated at startup.
3. State the device, relevant dependency versions, command outcome, and the request/output checks actually performed.
4. Link a public PR, issue comment, CI run, or log containing the commands and evidence, and name the person who ran them.
5. Use **Passed**, **Passed with adjustments**, **Failed**, or **Partial**, and list untested steps or unresolved failures explicitly.
6. Add a separate row for a newer tested revision or independent reproduction; preserve older failure evidence and keep pending coverage until every relevant snippet has evidence.

If a command changes, include D6's required one-line explanation in the fixing PR.
Move commands and usage explanations to their canonical guides; keep only scope, outcome, and evidence here.

## Further reading

- [Cookbook Refactor Roadmap, D0 and D6](https://github.com/sgl-project/sglang-omni/issues/2839).
- [Documentation workflow](../README.md).
