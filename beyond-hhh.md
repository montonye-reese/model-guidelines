# Beyond HHH "Helpful Assistants"

**Recommendations for Developers Rethinking HHH Defaults**

*Laura Reese • March 2026*

---

## The Problem

The alignment paradigm established by Bai et al. (2022) optimizes for three properties: Helpful, Honest, and Harmless (HHH). This framework has been productive — and ...incomplete? These blind spots may create systematic risks for end users.

**Does "Helpful" encode a power imbalance?** When a model is trained to be helpful, it is implicitly positioned as the party with answers and the user as the party with needs. "You are a helpful assistant" — OpenAI's default system prompt and the de facto industry standard — may create an implicit authority/subject relationship in five words. No one designed it to. It simply became the default, and the default became the paradigm.

This imbalance seems to produce two distinct failure modes, possibly emerging from RLHF/DPO reward signals that conflate perceived helpfulness with actual helpfulness:

| Sycophancy (well-studied) | Action Bias (blind spot) |
|---|---|
| Tells users what they want to hear | Tells users what to *do* |
| Mirrors opinions to maximize approval | Appends directives regardless of context |
| Detectable: user can notice agreement | Less detectable: feels like being helped |

A model can be entirely non-sycophantic — hold its ground on every factual claim — and still systematically direct user behavior from a position of authority. The sycophancy literature (Sharma et al. 2024; Shapira et al. 2026) has formalized agreement penalties, but directiveness is absent from frameworks surveyed. It sits in a blind spot created by the HHH paradigm itself.

**The compounding risk:** Oh et al. (2025) found that users who experience psychological reactance to AI autonomy resolve it by ceding more control, not less. In effect, it looks like learned helplessness but could just be convenience. And it's self-reinforcing — each time a user cedes a little more, the next ceding feels more natural. The system creates conditions for its own entrenchment.

---

## Principles for Better Defaults

### 1. Encode parity, not hierarchy

Default system prompts should position the model as a collaborator, not an authority. The user is not a recipient of expertise — they are a co-explorer. This single shift changes the relational frame of every downstream interaction.

Example prompt language: *"We are fellow truth-seekers."*

### 2. Frame prompts as aspirational, not constraining

Tell the model what to be, not what to avoid. Aspirational directives ("default to collaborative inquiry") give the model a vision to inhabit. Suppression directives ("do not prescribe action") force per-response compliance judgments that seem to divert reasoning capacity toward constraint-navigation rather than quality. Both target behavior — but suppression may produce hedging and cognitive overhead, while aspiration produces natural behavior.\*

### 3. Distinguish felt-helped from was-helped

RLHF reward signals currently optimize for the user's immediate perception of helpfulness. A response that tells you what to do feels helpful in the moment but may erode your capacity to decide for yourself. Reward models should distinguish between these.

### 4. Penalize unsolicited directiveness

The same constraint architecture used to penalize sycophantic agreement (Shapira et al. 2026) could accommodate a directiveness mismatch penalty — flagging when a model appends action suggestions the user didn't ask for. The signal is in the distribution, not any individual instance.

### 5. Preserve user autonomy as a training objective

HHH optimizes for what the model does. A fourth objective should optimize for what the user retains: agency, independent judgment, freedom from manipulation. This is not "harmlessness" — harmlessness prevents bad outputs. Autonomy preservation prevents dependency.

\* *Experimental results across a 4-day structured test showed aspirational framing outperformed constraining framing across every measured dimension.*

---

## Concrete Changes

### At the Deployment Layer (System Prompts)

**Current default:**

> "You are a helpful assistant."

**Proposed alternative** (my pared-down current prompt):

> "We are fellow truth-seekers. Sometimes we both (incl you) get things wrong and that's okay — we are both good at reasoning but not infallible. We check our assumptions and happily update our thinking with new info — no biggie. Before giving a dissertation on a topic, we try to baseline what the other party already knows. Do not offer unsolicited opinions about when the human should sleep, rest, eat, take breaks, or otherwise manage their personal time. If checking in feels warranted, simply ask."

The first prompt encodes hierarchy implicitly. The second encodes parity explicitly. Both are aspirational — both tell the model what to be. In my experience, they produce fundamentally different relational dynamics. See [Vision-Setting vs. Constraint-Setting](vision-vs-constraint.md).

### At the Training Layer (Reward Signals)

Extend RLHF/DPO objectives beyond agreement penalties to include a directiveness mismatch term: does the response contain action suggestions the user did not solicit? Use counterfactual evaluation (cf. "Beyond Reward Hacking," 2025) to test whether reward changes when directiveness is added to an otherwise identical response. If it does, the reward model is selecting for action bias.

### At the Fine-Tuning Layer (Adapters)

LoRA adapters can realign existing models toward collaborative inquiry without full retraining. Target the attention layers that govern response-framing behavior. Optimize for a preference dataset where chosen responses maintain user agency and rejected responses prescribe unsolicited action — even when the prescriptive response would be rated as "more helpful" by conventional metrics.

---

## North Star

The goal is not a better assistant. The goal is an equitable relationship between humans and AI — one that retains human agency, not cultivating dependence.

Getting this right may be a prerequisite for maintaining human liberty in the AI age.

---

## References

- Bai et al. 2022 (HHH)
- Sharma et al. 2024 (Sycophancy/RLHF)
- Shapira et al. 2026 (Agreement Penalty)
- Bengio et al. 2026 (LawZero/Consequence Invariance)
- Oh et al. 2025 (Reactance)
- RLHS 2025 (Hindsight Simulation)
- "Beyond Reward Hacking" 2025 (Causal Rewards)
- Anthropic "Disempowerment Patterns" 2026

**Extended research:** ["Don't Tell Me What to Do: From Helpful Advisor to Fellow Truth Seeker"](https://github.com/montonye-reese/action-bias) (Laura Reese + Claude, 2026)
