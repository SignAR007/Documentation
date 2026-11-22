# Validation Gates and Schedule Contingencies

Use these gates with the six-week timeline. A gate is complete only when its evidence is recorded; if it fails, move the dependent milestone instead of skipping the check.

| Gate | Target | Evidence required | If blocked |
|---|---|---|---|
| Feature contract | Week 2 | Training and decoder agree on schema version, dimensions, sequence length, labels, and unit metadata. | Keep the existing compatible model active and resolve the mismatch before training or transfer. |
| Dataset readiness | Week 4 | Consent and label review are complete; signer coverage and dataset checks meet the active validation profile. | Collect or review more data and shift candidate evaluation. Live-camera samples are optional and are not a readiness gate. |
| Candidate decision | Week 5 | Held-out evaluation, per-label results, stress checks, and human review are recorded for the exact candidate. | Defer promotion and document the failed criteria; do not present an unevaluated candidate as ready. |
| Release check | Week 6 | Active vocabulary, metadata, model, weights, and offline cache agree; privacy and deployment boundaries are checked. | Keep the current release and move the release milestone until the artifact passes review. |

### Schedule risks

- **Data collection takes longer than planned:** protect consent, label review, signer coverage, and evaluation time. Reduce optional interface polish before reducing validation.
- **A holistic candidate fails evaluation or compatibility:** leave the current compatible model in place, record the gap, and schedule another candidate cycle.
- **A sign reviewer or target device is unavailable:** mark the affected check as blocked and reschedule it. Do not treat an unchecked sign form or device as verified.
- **Release assets drift during integration:** compare the manifest and service-worker asset list against the exact model files before release.
