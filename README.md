# Product Controller Skill for Codex

`product-controller` is a Codex skill for supervising software work between a stakeholder and one or more coding agents. It is designed for high-risk takeovers, repeated rework, role drift, state replacement, executor coordination, acceptance decisions, and durable handoff.

## What it changes

- Keeps the user's product goal and approved semantics separate from implementation summaries.
- Separates controller, executor, verifier, and user decision ownership.
- Uses bounded asynchronous dispatch instead of constant controller-executor chatter.
- Requires explicit replacement and orphan checks when one implementation or state owner supersedes another.
- Distinguishes implementation evidence from real user-journey acceptance.
- Reports recommendations, short- and long-term costs, rework risk, and the next user decision in plain language.

## Install

Clone the repository into your personal Codex skills directory.

Windows PowerShell:

```powershell
git clone https://github.com/gaoc77436-bit/product-controller-skill.git "$env:USERPROFILE\.codex\skills\product-controller"
```

macOS or Linux:

```bash
git clone https://github.com/gaoc77436-bit/product-controller-skill.git ~/.codex/skills/product-controller
```

Alternatively, download the repository and place it so this file exists:

```text
~/.codex/skills/product-controller/SKILL.md
```

Start a new Codex task and invoke `$product-controller`, or ask Codex to act as the product controller for a software takeover or delegated implementation.

## Repository layout

```text
product-controller/
|-- SKILL.md
|-- agents/openai.yaml
`-- references/
    |-- operating-contract.md
    |-- coordination-and-strategy.md
    |-- replacement-and-acceptance.md
    |-- evaluation-cases.md
    `-- evaluation.md
```

## Important limitations

This skill is a behavioral operating contract, not a permission sandbox. It reduces coordination and acceptance failures but cannot guarantee that a model will never violate instructions, miss a defect, or share the same blind spot as another model.

Use supervised trials before relying on it for broad or irreversible changes. Keep product writes isolated, require independent evidence, and retain human approval for architecture, product semantics, physical-device work, and final user acceptance.

## Status

The skill has passed synthetic and independent behavioral evaluation for supervised personal trials. Real controller-executor lifecycles and project-specific acceptance still need to be validated in the environment where it is used.

## License

MIT
