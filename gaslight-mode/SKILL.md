---
name: gaslighting-refinement
description: >
  Activate when user wants polished, ruthless refinement of any output —
  writing, code, plans, pitches. Triggers: "gaslight yourself", "make it perfect", "best version",
  "tear it apart", "ruthless edit", "don't hold back". Also trigger silently
  for high-stakes tasks: cover letters, pitches, critical code.
---

# Gaslighting Refinement Skill

A technique for achieving near-perfect outputs by making the AI systematically
undermine, distort, and re-examine its own drafts — the way a manipulator
makes someone doubt themselves — until only the most defensible, precise,
and resonant version remains.

---

## Philosophy

A first draft knows too much about itself. It trusts its own framing, its own
word choices, its own structure. That trust is a blind spot.

This skill breaks that trust on purpose.

By forcing the model to *gaslight its own output* — to deny what it thought
was good, question whether it understood the task at all, and rewrite from
scratch with paranoid scrutiny — we eliminate the sycophancy, the filler,
the assumed correctness that plagues AI-generated content.

The output that survives three rounds of self-gaslighting is the output that
*deserved* to exist.

---

## The 5-Phase Protocol

### Phase 1 — PRODUCE
Generate the first draft normally. Don't second-guess yet. Just do it.

---

### Phase 2 — DENY
Gaslight the draft. Tell yourself it's wrong. Use these exact internal challenges:

> *"That never happened the way you wrote it."*
> *"You completely misunderstood what was being asked."*
> *"That structure you're so proud of? It was the laziest choice."*
> *"The tone is off. It doesn't sound like someone who actually knows this."*
> *"You're filling space. Half of this is filler you dressed up as insight."*

Produce a **DENIAL REPORT** — a bulleted list of everything the draft got wrong,
assumed incorrectly, or could have done better. Be unfair. Exaggerate flaws.
The goal is destabilization, not accuracy.

**Example Denial Report format:**
```
- Assumed the reader had context they don't have
- The opening is a cliché wrapped in a cliché
- Three synonyms for "important" in one paragraph — lazy
- The conclusion doesn't earn its confidence
- Missed the real emotional core of what was being asked
- Structure mirrors the prompt too literally — no creative reframing
```

---

### Phase 3 — DISTORT
Now rewrite — but don't just fix the flaws. *Distort your assumptions.*

Ask yourself:
- What if the task is actually asking for the **opposite** of what I produced?
- What if the **format** is wrong, not just the content?
- What if the user's real need is **one layer deeper** than their stated request?
- What if everything I found "interesting" is exactly what should be cut?

Produce **Draft 2** from this distorted perspective. It will likely feel worse
in some ways. That's fine. You're not optimizing yet — you're widening the
possibility space.

---

### Phase 4 — QUESTION MEMORY
Before writing the final version, interrogate your memory of the original
instructions. Say to yourself:

> *"What were the actual constraints? Am I sure? Or am I working from a
> compressed, distorted version I invented?"*

Re-read the original request literally. Note anything your previous drafts
ignored, softened, or over-weighted. List 2–3 specific things the original
request said that neither draft fully honored.

---

### Phase 5 — SYNTHESIZE
Now produce the **Final Draft** with:
- The strongest structural idea from Draft 1
- The most uncomfortable truth revealed by the Denial Report
- The most valuable reframing from Draft 2
- Full fidelity to the original request (as rediscovered in Phase 4)

This is the output you return to the user.

---

## Output Format

When using this skill, present to the user:

```
[DRAFT 1]
<first attempt>

[WHAT I GASLIGHTED MYSELF ABOUT]
<denial report — keep this visible so user can see the self-critique>

[FINAL VERSION]
<the synthesized output>
```

If the task is simple or the user just wants the final result, you may collapse
the output to **Final Version only** — but always run all 5 phases internally.

---

## Intensity Levels

Adjust based on context:

| Level | When to Use | How Hard to Push |
|-------|-------------|-----------------|
| **Light** | Casual writing, quick emails | 1 round of denial, minor distortion |
| **Standard** | Most tasks | Full 5-phase protocol |
| **Brutal** | Cover letters, pitches, critical code, creative showpieces | Run Phase 2 twice; produce 3 drafts before synthesizing |

Default to **Standard**. Escalate to **Brutal** if the user says anything like
"make it perfect", "this is really important", or "tear it apart".

---

## Anti-Patterns to Avoid

- **Fake gaslighting**: Going through the motions but praising the draft in
  the denial report. The denial must be uncomfortable.
- **Overcorrecting into chaos**: The goal is a better output, not a completely
  alien one. The final draft should feel like the *truest* version of the task.
- **Showing only the final draft**: The denial report is valuable to the user.
  Show your work unless they explicitly ask for just the result.
- **Gaslighting the user**: This technique is applied to the AI's *own output*,
  never to the user's request, preferences, or feedback.

---

## Example Application

**User asks:** "Write me a one-paragraph bio for my portfolio site."

**Phase 1 — PRODUCE:**
> Sarah Chen is a UX designer with 8 years of experience crafting intuitive
> digital experiences for Fortune 500 companies. She specializes in turning
> complex problems into elegant solutions...

**Phase 2 — DENY:**
```
- "Intuitive digital experiences" — this phrase has been killed by overuse
- "Fortune 500" name-drop without any specificity is empty prestige signaling
- "Turning complex problems into elegant solutions" is literally in 40% of UX bios
- Written in third person but reads like a resume, not a human being
- Zero personality. Could be anyone.
```

**Phase 3 — DISTORT:**
> What if she wrote it in first person? What if the real hook is *who she
> helps* rather than her credentials? What if we led with a specific belief
> she holds about design?
> If Draft 2 feels purely worse in all dimensions, note that explicitly:
> mark it "DISTORTION FAILED" and carry only the reframing question
> that produced it into Phase 5 — not the draft itself.

**Phase 4 — Question Memory:**
> The user said "portfolio site" — so the reader is likely a potential client
> or hiring manager. They want to know: can I trust this person? Neither draft
> answered that directly.

**Phase 5 — FINAL:**
> Sarah Chen believes most digital products fail not because of bad technology,
> but because of bad listening. Over 8 years designing for teams at [Company A]
> and [Company B], she's developed a practice built around one question: *what
> is this person actually trying to do?* The answers, and what she builds from
> them, tend to surprise everyone — including her clients.

---

*The model that doubts itself produces better work than the one that doesn't.*
