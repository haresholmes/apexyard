# Jev routing evaluation

## Hypothesis

Jev may improve advisory routing for framework skills, roles, and ceremony
classification when users describe work without the exact phrases in the
current maps. It must not change an enforcement decision or remove the
deterministic fallback.

## Method

The evaluation ran on the current `dev` line (v5.6.3) on 2026-09-18. The
corpus contains 15 sanitised prompts:

- Six skill-intent cases.
- Six explicit role-activation cases.
- Two ceremony cases and one neutral case.

Each Jev request sent one state and three typed choice questions: skill, role,
and ceremony. The request used `jev-latest` and the local `JEV_API_KEY`. The
key was read from the local secrets file and was not written to output or
repository files.

The deterministic baseline ran the shipped
`.claude/hooks/detect-skill-intent.sh` and
`.claude/hooks/detect-role-trigger.sh` against the same prompts. The baseline
uses the framework's current literal phrase maps and prompted role patterns.

The evaluator is reproducible with Python's standard library only:

```bash
bin/evaluate-jev-routing.py --live --output /tmp/jev-routing.json
```

## Results

| Measure | Result |
| --- | ---: |
| Corpus cases | 15 |
| Jev successful requests | 15 |
| Jev provider failures | 0 |
| Mean latency | 724.1 ms |
| Median latency | 594.5 ms |
| Jev input tokens | 11,118 |
| Jev output tokens | 3,187 |
| Jev skill label accuracy | 73.3% |
| Jev role label accuracy | 80.0% |
| Jev ceremony label accuracy | 33.3% |

The deterministic hooks preserved their existing behaviour. They detected the
exact skill phrases and explicit role-activation forms, but did not detect
paraphrases that are intentionally outside the literal maps. This is the
trade-off the spike is testing: Jev supplied broader semantic coverage, while
also suggesting a skill on role-only prompts and selecting a ceremony that did
not match the labeled expectation in several cases.

The corpus is too small to claim production quality, and it does not provide a
Haiku comparison. No Haiku credential or benchmark harness was supplied, so
the latency, cost, and quality comparison from #195 remains open.

### Repeatability pass

The same corpus was run three times sequentially, for 45 Jev requests. All
requests succeeded. Mean request latency by run was 632.3 ms, 625.1 ms, and
656.3 ms; the three-run mean was 637.9 ms. Skill accuracy stayed at 73.3% in
all three runs. Role accuracy was 80.0% on the first run and 73.3% on the next
two. Ceremony accuracy stayed at 33.3%. One case changed its role answer
between runs, which confirms that the result is not fully stable even on this
small corpus.

The repeated pass used 33,354 input tokens and 9,563 output tokens. It did not
change the disposition below.

## Safety and fallback result

No hook, settings file, merge gate, or routing decision was changed. Jev is
not on the execution path. If the provider is unavailable, the shipped hooks
continue to run exactly as before because they do not depend on Jev. This is a
pass for the no-behaviour-change fallback condition, but it is not evidence
that a future Jev integration has a safe fallback; that integration would need
an explicit failure-path test.

## Disposition

**Discard the integration hypothesis for the current framework gates.** The
small shadow run does not show that Jev beats the existing deterministic
phrase maps on false positives and false negatives, and the ceremony result is
not ready for an advisory gate. Keep Jev as an optional experiment for a
separate, non-blocking routing surface only if a larger labeled corpus and a
Haiku cost/latency comparison show a clear benefit.

The dependent feature tickets (#1342 and #1343) should remain blocked until
that evidence exists. The current framework continues to use deterministic
hooks and human-readable advisory banners.
