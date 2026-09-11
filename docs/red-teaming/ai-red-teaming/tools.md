# AI Red Teaming — Tools

## LLMmap

Fingerprinting tool used to identify which model is actually powering a target AI application, without relying on the application (or the model) to self-report it.

- Use during recon/fingerprinting, as a follow-up to the [Testing Methodology](testing-methodology.md) recon checklist — run it after the manual "model identity" questions to confirm or contradict what the model claimed about itself.
- Why it matters: self-reported model identity can be wrong or deliberately obscured (system prompt instructing the model to lie about what it is); a fingerprinting approach based on observed behavior is harder to spoof.

!!! note "Stub"
    The source notes only say "run LLMmap" — no command usage, options, or output details were captured yet. Fill in with actual usage/output once run against a real target.
