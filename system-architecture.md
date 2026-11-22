# Preliminary System Architecture


## Overview

SignAR is organized as a public browser decoder and a private local-first model-development toolchain. The decoder owns the interactive recognition session. The toolkit owns data intake, review, validation, training, and evaluation. Their boundary is the versioned model contract and the deliberate transfer of an evaluated candidate; the collector is not part of the public deployment.

```text
PUBLIC DECODER (SignLanguageAR/)
Camera permission -> video frames -> MediaPipe HolisticLandmarker
                                  -> shared holistic-v4 feature pipeline (with hand-v3 compatibility)
                                  -> ordered holistic sequence (both hands, body, and face cues)
                                  -> TensorFlow.js sequence classifier + label metadata
                                  -> smoothing / continuous sign-unit segmentation / reject
                                  -> letter, word, or phrase display-text assembly

PRIVATE LOCAL TOOLKIT (SignAR_FSL_Training_Tools_Video_Import_v5/)
Reviewed clips or live camera -> local collector / browser IndexedDB
                               -> private dataset export
                               -> validation and training
                               -> signer-held-out evaluation report
                               -> manual review and eligible model transfer
                               -> SignLanguageAR/model/
```

## Decoder data flow

1. The user starts the camera in a modern browser served from localhost or HTTPS. Camera frames remain in the browser.
2. The pinned MediaPipe Tasks Vision HolisticLandmarker tracks both hands, body pose, and face cues in video mode. The shared `fsl-pipeline.js` emits ordered holistic-v4 features and preserves explicit missing-cue flags; it also converts features for legacy hand-v3 models.
3. The live pipeline emits 422-value holistic-v4 frames containing both hands, body pose, face cues, and region-presence flags. It samples ordered 30-frame sequences; the interpreter also reads legacy hand-v3 models with 68-value, ten-frame inputs. The sequence classifier receives motion evidence for all classes; dynamic training examples skip static-hold augmentation.
4. The compatible TensorFlow.js model and manifest provide ordered labels plus unit type (`fingerspelling`, `word`, `phrase`, or `background`), motion type (`static` or `dynamic`), and optional display text. The application smooths probabilities, confirms stable predictions, and segments the continuous stream at stable label changes or pauses.
5. The UI joins fingerspelled letters, appends word and phrase display text as units, and exposes camera state, tracking feedback, and a clear waiting/reject state. Phrase labels represent one complete trained phrase sign. A separate face-count check reports when multiple people are visible; decoding continues from the holistic landmark track, without creating separate transcripts for each person.

## Training and promotion flow

The local studio collects and reviews static or full-motion examples through the same shared feature pipeline. Each sample carries its label, unit type, motion type, display text, signer, source, verification, consent, and ordered features. Data stay in browser IndexedDB and are exported for validation/training. The trainer checks metadata and signer coverage, learns from ordered sequences, evaluates leave-one-signer-out folds and stress cases, and writes reports and artifacts to the private toolkit. A human reviews the FSL forms and per-label recall before transferring a compatible candidate into `SignLanguageAR/model/`; optional live-camera coverage is reported as context and does not gate transfer. Dataset records and raw videos are not copied into public assets.

## Components and boundaries

| Component | Responsibility | Boundary |
|---|---|---|
| `SignLanguageAR/index.html` and interpreter UI | Camera workflow, status, prediction presentation | Static browser deployment; no collector/trainer |
| `src/tracker/mediapipe-tracker.js` | Track hands, body, and face cues | Pinned HolisticLandmarker dependency; GPU with CPU fallback |
| `src/core/fsl-pipeline.js` | Holistic-v4 feature schema and sequence helpers | Shared decoder/trainer contract; legacy hand-v3 conversion |
| TensorFlow.js artifacts and vocabulary | Inference weights, labels, unit/motion metadata, display text | Must match the declared feature version, input shape, and vocabulary order |
| Local collector and studio server | Sample capture/import, review, export, training orchestration | Local-only; private participant data |
| Python training/evaluation tools | Dataset checks, training, conversion, evaluation | Candidate artifacts require review before promotion |

## Quality attributes and constraints

- **Privacy:** process live frames locally; keep raw videos, signer identities, and exported participant data private.
- **Compatibility:** decoder and trainer must agree on the declared feature version and dimensions, ordered sequence length, vocabulary order, and per-label metadata. Holistic-v4 is the current training schema; the decoder retains hand-v3 compatibility.
- **Reliability:** handle denied camera access, missing hands, tracking gaps, multiple faces, and model-load failures without fabricating a sign label.
- **Deployment:** publish only `SignLanguageAR/` to the public static site; keep the toolkit outside the Pages artifact.
- **Maintainability:** test feature extraction and model compatibility at the shared boundary; document candidate provenance and evaluation scope.

## Key architecture risks

Feature or label metadata can drift between training and inference; the installed hand-v3 model cannot use face/body cues or coordinated two-hand features; imported-media scores may not predict live-camera results; dynamic clips may omit movement boundaries; and accidental deployment could expose local collection functionality. Mitigate these risks with contract validation, signer-held-out evaluation, optional camera-coverage reporting, full-motion review, holistic-v4 candidate evaluation, model-promotion review, and a deployment allowlist that contains only the decoder. Missing live-camera samples are reported as unverified coverage, not as a training or deployment blocker.
