# Preliminary Technology Stack


## Browser application

| Area | Technology | Role in SignAR |
|---|---|---|
| Application UI | HTML, CSS, and browser JavaScript modules | Static, installable browser experience for the camera decoder |
| Holistic tracking | MediaPipe Tasks Vision HolisticLandmarker, pinned browser assets | Captures both hands, face, and body cues; GPU is attempted with CPU fallback |
| Feature processing | Shared `signar-fsl-holistic-v4` pipeline | Encodes both hands, 33 body landmarks, 48 face cues, and presence flags in 422-value frames |
| Model inference | TensorFlow.js sequence model artifacts | Classifies supported fingerspelling, word, phrase, static, and dynamic labels from ordered model features |
| Camera and rendering APIs | `getUserMedia`, video/canvas, service worker and web app manifest | Local camera flow, landmark overlay, installability, and static asset caching |
| Static hosting | GitHub Pages or another HTTPS-capable static host | Serves only the `SignLanguageAR/` public application |

## Local collection and training toolkit

| Area | Technology | Role in SignAR |
|---|---|---|
| Collection interface | HTML, CSS, and browser JavaScript | Captures/reviews samples and stores them locally in browser IndexedDB |
| Local API/studio | Python standard-library server tooling | Serves the local studio and invokes the local training workflow; not a public service |
| Training | Python 3.10?3.12 and pinned TensorFlow/tool dependencies | Trains holistic-v4 sequence classifiers; migrates supported hand-v3 and legacy samples |
| Evaluation | scikit-learn 1.4.2 plus project evaluation scripts | Computes held-out-signer classification and readiness metrics |
| Model conversion | TensorFlow.js converter 4.22.0 where supported by the platform | Exports model artifacts for browser inference |
| Windows toolchain | Project-pinned JAX/JAXLIB, TensorFlow Hub, importlib-resources, and setuptools constraints | Supports the current Windows training/conversion install path |
| Checks | Python unittest-style tests, Node-based tests, and Prettier check | Protects feature, tracker, model compatibility, trainer, and formatting behavior |

Exact versions should be checked against `requirements.txt`, package manifests, and the project's test scripts when the final report is submitted. The browser runtime and Python training environment are distinct; Python packages are not required to run the public decoder.

## Data and model contract

The decoder and training toolkit share a holistic-v4 feature contract: 422 values per frame representing both hands, 33 body landmarks, 48 face cues, and four presence flags, sampled as ordered 30-frame inputs. The decoder retains compatibility with the installed 68-value hand-v3 model. The manifest defines label order, unit type, motion type, and optional display text. Continuous segmentation closes recognized units at stable label changes or pauses; phrase labels represent complete trained phrase signs. Training output must match the declared schema and browser model loader before transfer.

## Rationale and trade-offs

- Browser-side inference avoids sending camera frames to a server and makes the decoder suitable for static hosting.
- A shared feature pipeline reduces schema drift between collection and live inference.
- A local Python toolkit supports reproducible training/evaluation without exposing participant data through the public site.
- A static deployment is simple to host, but camera access requires localhost or a secure HTTPS context and compatible browser/device support.
- TensorFlow.js conversion and Windows package constraints add setup complexity; pinning dependencies and testing on the target operating system reduces environment surprises.

## Security, privacy, and operations

Use only consented, reviewed examples. Keep raw recordings, identifying information, signer aliases that could identify a participant, and exported feature datasets out of the public repository. Commit source code, tests, and approved model artifacts only. Review model artifacts and evaluation reports before promotion. Pin external browser assets and dependencies where the project already does so; validate upgrades with compatibility tests before merging.
