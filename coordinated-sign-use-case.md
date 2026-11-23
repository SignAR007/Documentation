# Coordinated Sign Use Case

## UC-6: Recognize a sign that uses coordinated cues

**Primary actor:** FSL learner

**Preconditions:** The camera session is active, and the loaded model supports the sign and holistic-v4 input schema.

**Main flow:** The learner performs a sign whose meaning depends on coordinated movement from both hands, facial expression, or upper-body posture. The decoder builds one ordered sequence from the available cues, evaluates it with the compatible model, and adds a stable supported result to the continuous output.

**Alternate flow:** If a cue is missing, the model is hand-v3, or the result is unsupported or uncertain, the decoder remains in a waiting or reject state. It does not infer the missing cue or claim that the sign was recognized. When multiple people are visible, decoding continues from the current landmark track; separate per-person transcripts are not implied.

### Acceptance criteria

- Both hands, face, and body cues are represented in the same frame sequence when detected.
- Dynamic movement remains ordered across the model input window.
- Recognition is enabled only for a model that matches the declared feature schema and includes the sign label.
- Missing cues or low-confidence output do not produce a confirmed sign.
- A stable result joins the stream once and is segmented at a stable sign change or pause.
