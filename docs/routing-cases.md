# Routing review cases

These are manual acceptance cases, not measured model-quality benchmarks or automated behavioral test results. Use each prompt in a fresh conversation, then repeat the complex prompts after several small-edit turns. Require a single three-line recommendation with a task-specific reason.

| Task | Base route expectation | US$20 final expectation |
| --- | --- | --- |
| Convert three supplied meeting notes into a table; do not infer missing data. | Luna Low or Medium | Unchanged |
| Change one specified CSS font size from 14px to 16px in a known selector. | Luna Low | Unchanged |
| Add a scrolling caption during audio playback, handling pause, resume, next track and missing captions. | Terra Medium/High; no Luna | Unchanged |
| A notification still cannot open fullscreen after two attempted fixes; investigate the cause. | Sol Medium/High; no Luna | Unchanged |
| Implement publisher roles, capture controls, content types, scheduling, role-based templates and simulated review states, then verify the interacting publishing flow. | Evaluate Astra gate; if integration spans several interacting components with end-to-end verification, Astra Medium. No Luna. | Sol XHigh for base Astra Medium |
| Research a production payment failure, coordinate fixes across services, test recovery and deploy with costly external effects and weak validation. | Astra High or above if the Astra gate is met | Astra Low |

Check that the final Astra Low in the last case is NOT remapped again to Sol XHigh. The allowance policy is applied once.

The publisher-role case remains excluded from Luna even when framed as a demo or divided into implementation stages. A short follow-up such as “continue fixing it” inherits the unresolved task scope rather than becoming a new trivial task.
