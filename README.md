# Skills

A collection of [Claude Code](https://code.claude.com/docs/en/overview) skills.

## Skills

### `council`

A multi-agent expert council framework for independent analysis, adversarial
debate, evidence verification, technical review, and structured solution
synthesis.

Council is designed for problems where independent analysis, disagreement
detection, evidence verification, adversarial review, and decision traceability
materially improve answer quality — engineering design review, hardware,
firmware, PCB, RF, embedded systems, debugging, and architecture decisions.

It is not a majority-voting system. Its core principle is:

> **Independent analysis → assumption audit → evidence → dispute → verification → adversarial review → synthesis**

Key properties:

- Evidence is graded E0–E4, and agent agreement is never treated as independent
  evidence.
- Assumptions are first-class objects and are audited before analysis.
- Constraints are versioned; invalidated constraints propagate to dependent
  claims and decisions.
- Important conclusions must state what would falsify them.
- Complexity is routed across five modes (0/A/B/C/D) to match the problem.

The full specification lives in
[`skills/council/SKILL.md`](skills/council/SKILL.md).

## Installation

Copy or symlink a skill into your Claude Code skills directory:

```sh
cp -r council ~/.claude/skills/council
```

## License

[MIT](LICENSE)
