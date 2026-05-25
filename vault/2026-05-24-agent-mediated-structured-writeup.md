# Agent-mediated structured writeup — encode domain conventions, accelerate first draft

**Pattern:** Domain-specific structured-writeup tasks (RFI / RFP / vendor questionnaire, security review responses, customer briefing docs, technical blogs, internal policy updates, performance review writeups) benefit from a *dedicated agent* that encodes the author's domain conventions and produces a first-pass draft. The author moves from "writing from scratch" to "iterating on a well-structured first draft." Worked example: an RFI agent compressed a ~2-week historical baseline into ~30 minutes elapsed, with the author judging the output's quality higher than past manual attempts.

**Why it matters:**

- **Most structured-writeup time is convention-encoding, not knowledge production.** The author already knows what should be there. Section structure, phrasing conventions, reusable answer skeletons, what reviewers actually check for — that's the bulk of the work, and it's all encodable.
- **The agent is a shape, not a writer.** Generic "writing assistant" framings fail because they're too broad to be useful. Value comes from the *domain encoding* — the agent's job is to know what an RFI / blog / brief / questionnaire *looks like* in this author's voice and this reviewer's expectations.
- **Author expertise is the prerequisite, not the substitute.** This pattern accelerates experts; it doesn't manufacture them. An agent encoded by someone without domain depth will replicate their blind spots at speed.
- **Compression appears multiplicative, not additive.** The headline result (30 min vs 2 weeks) is not a 30% speedup — it's an order-of-magnitude shift, because the agent eliminates the "starting from blank" cost entirely and the author's role becomes pure iteration.

---

**What a good agent encodes** (drawing from the RFI worked example):

1. **Section structures and orderings** for the writeup type — the canonical shape reviewers expect.
2. **Phrasing conventions and anti-patterns** — house style, what to avoid, what raises red flags with reviewers.
3. **Common questions / re-usable answer skeletons** — for high-frequency question types (security, compliance, deployment, integrations, SLA), the answer shape is well-known.
4. **Reviewer expectations** — what makes a section pass vs. fail review. Often unwritten in the docs the author would consult manually.
5. **Pointers to canonical references** — the docs, runbooks, prior-response archives, or product specs the author would pull from manually.

The agent is a structured version of "what I'd tell a junior person to read before they wrote this."

---

**When this fits:**

- Output is *structured* — there's a template, required sections, or known reviewer expectations.
- Author has *domain expertise* the agent can encode.
- Quality is *judgeable* — reviewer accepts or rejects against the conventions.
- Frequency is moderate-to-high (1+/month) **OR** a single instance is high-stakes enough to justify the agent investment.

**When this doesn't fit:**

- Output is creative or open-ended — no template means nothing to encode.
- Author lacks domain expertise — the agent has no patterns to follow and will hallucinate confidently.
- Truly one-off task where building the agent costs more than doing the task manually once.
- Bottleneck is *research*, not *writeup* — the agent doesn't help if the hard part is discovering what to say.

---

**HITL pattern (OPEN — under study):**

The right amount of human-in-the-loop intervention is the unsolved question for this pattern. Too much HITL collapses the leverage (author writes everything anyway, just slower). Too little risks polished-but-wrong output reaching reviewers, plus cost runaway if the agent iterates against itself unsupervised.

The candidate harness experiment (calibrating HITL gradient as the actual independent variable, measuring cost-per-output against a manual baseline) is the right shape to answer this. **Update this section when data exists.**

Open sub-questions:

- Is the right HITL rate per-writeup-type, per-author, or per-task-instance?
- Does quality degradation appear gradually as HITL drops, or as a cliff?
- Does the answer change when the author *isn't* a deep expert? (Falls back to "don't use this pattern" or does some shallow encoding still help?)

---

**Other candidates for the same pattern:**

- Technical blogs (one agent per blog type — vendor blog, dev tutorial, opinion piece)
- Customer briefing docs / pre-call prep notes
- Internal policy or process documents
- Security review responses (SOC 2, ISO, GDPR controls questionnaires)
- Patent disclosures / IP-review writeups
- Performance review writeups (manager perspective)
- Conference talk proposals / CFP submissions
- Onboarding doc series (same author, multiple consistent structured docs)

The shape generalizes anywhere there's a *recurring structured deliverable* with *encoded reviewer expectations*.

---

**Anti-patterns:**

- **Trying to encode a domain the author hasn't mastered.** Teaching the agent the author's blind spots at speed.
- **"Generic writing assistant" framing.** Too broad to be useful. The agent's value is in the domain encoding, not the writing — strip the generic-ness.
- **Skipping HITL early.** No calibration data, risk of polished-but-wrong output. Start high-HITL, relax with measurement.
- **One agent for many writeup types.** The encoding is what makes it valuable — sharing one agent across RFI, blog, and brief writeups dilutes each.
- **Building the agent before doing the task manually a few times.** The conventions to encode aren't known yet. Do 2-3 manual passes first, *then* extract the encoded version.

---

**Evidence honesty:**

- **N=1 production agent**, private, built and used by the author.
- **N=2 runs through 2026** — vendor questionnaires are infrequent in the author's current role (~2/year).
- **Author's pre-existing expertise:** multi-year prior background in vendor-questionnaire response work, which validates the conventions the agent encodes. The agent's quality is downstream of that expertise, not a substitute for it.
- **Baseline:** ~2 weeks of historical manual work for comparable questionnaires.
- **Result:** ~30 minutes elapsed wall-clock, higher quality than past manual attempts per author's judgment.

Caveats: per-author judgment, not blind reviewer comparison. Run count is low. The pattern's generalization to other structured-writeup types is theoretical until validated on a second instance. Promote to `confirmed` after one more independent applicable case.

---

**How to apply:**

- **Pick a writeup type the author has done well, manually, multiple times** — that's where the conventions are mature enough to extract.
- **Encode the structure first, the phrasing second.** Structure carries 60–70% of the leverage. Phrasing polish is the long tail.
- **Build the agent during the *next* manual run** — the author is already in the right context.
- **Iterate with high HITL initially.** Use the first 2–3 runs to refine the encoding. Drop HITL only after the agent has produced acceptable output unaided on a representative task.
- **Measure cost-per-output and quality from the first run.** Without baseline data, the pattern's value is anecdotal and not transferable to other authors or contexts.

---

**Status:** seed.

Promote to `confirmed` when (a) the pattern is applied successfully to a second writeup type, or (b) the HITL-calibration question gets data from the candidate harness experiment.

Related:
- [[2026-05-23-handoff-pattern-structured-context-transfer]] — also exploits encoded conventions (10-section handoff structure). Both patterns are forms of *structured artifact + author-supplied content* leverage.
- [[feedback-pattern-domain-specificity]] — promotion to `confirmed` waits for a second independent applicable case, per the cross-validation rule.
