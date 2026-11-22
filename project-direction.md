# Proposed Final Project Direction


## Project direction

SignAR will be developed as a Filipino Sign Language (FSL) learning and sign-recognition system. Its browser decoder will use a device camera to recognize supported fingerspelling letters, isolated word signs, phrase signs, and static or dynamic sign sequences, then present stable, model-produced labels and configured display text. The interface presents supported sign labels and configured display text; it does not replace an interpreter or provide open-domain translation. The installed model weights currently cover the existing fingerspelling vocabulary; additional classes become recognizable only after their forms are collected, reviewed, trained, evaluated, and deployed.

The project has two related but deliberately separated components. The public-facing `SignLanguageAR/` application provides the decoder and learning-oriented interface. The local-first training toolkit provides reviewed sample collection, dataset validation, model training, and candidate evaluation. Only a reviewed and compatible model should move from the toolkit to the decoder. Collection and training controls, raw recordings, and participant datasets must remain outside the public deployment.

## Intended outcome

The system provides an end-to-end workflow: a user opens the decoder, grants camera permission, performs supported signs continuously, and sees stable labels and display text after tracking and confidence checks. Holistic-v4 frames represent left and right hands together with face and body cues; ordered windows preserve dynamic movement. The streaming decoder splits recognized units when a different label stabilizes or a pause marks a boundary. Fingerspelled letters join as text, while word and phrase units enter as complete display-text tokens. This boundary layer segments the model's supported labels; it does not infer untrained signs or translate FSL grammar. The user can distinguish recognized output from waiting, rejected, or camera-unavailable states. Collection and evaluation tools avoid exposing private participant media.

## Scope

**Included:** browser-based camera input; holistic tracking and versioned feature extraction for both hands, face, and body; label metadata for fingerspelling, word, phrase, background, and static/dynamic units; ordered temporal modeling for dynamic signs; continuous sign-unit segmentation at stable label changes and pauses; display-text output assembly; prediction smoothing and release behavior; an installable static web experience; a private local collection/training workflow; signer-held-out evaluation and documented limitations.

**Excluded from the current system:** unrestricted conversational sign-language understanding, automatic semantic translation of arbitrary FSL sentences, claims of production-grade accuracy, cloud upload of camera frames or raw participant recordings, and deployment of the training studio as part of the public website. A phrase label represents one trained phrase sign, and continuous segmentation operates over trained labels rather than translating arbitrary conversation.

## Success criteria

The project is successful when the public decoder can be run over localhost or HTTPS, obtains camera permission with clear feedback, loads a compatible sequence model, and provides stable sign predictions or an explicit reject/wait state. The training workflow must preserve unit and motion metadata, validate signer and label coverage, evaluate held-out signers and stress cases, and report optional camera coverage as context. Camera samples are not required for training or transfer. Documentation must state which labels the active model supports, its evaluation dataset, acceptance profile, privacy boundary, and unverified cases. Evaluation results reflect the dataset and conditions described in the report; performance outside that coverage remains unverified.

## Assumptions and risks

Recognition quality depends on camera framing, lighting, cue visibility, sign execution, and signer diversity. The interpreter keeps the installed hand-v3 model usable while the tracker, collector, and training pipeline support holistic-v4; newly captured two-hand, face, and body cues become recognizable after a matching holistic model is trained, evaluated, and deployed. Imported-media evaluation may not predict live-camera performance. FSL forms and labels require review by a qualified or otherwise appropriate FSL reference. The team must preserve participant consent and data minimization, keep identifying information and raw recordings private, and review changes to model artifacts before deployment. The exact final supported vocabulary and test-device matrix should be confirmed by the group before submission.
