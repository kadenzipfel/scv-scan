# SCV Scan

A Claude Code skill that scans Solidity codebases for security vulnerabilities by referencing 36 unique vulnerability types source from [smart-contract-vulnerabilities](https://github.com/kadenzipfel/smart-contract-vulnerabilities).

## Setup

1. Clone this repo into your Claude skills directory:

```bash
git clone <repo-url> ~/.claude/skills/scv
```

2. Run the skill in your codebase

```bash
cd my_repo
claude
/scv
```

## How It Works

The skill follows a four-phase audit workflow:

1. **Load Cheatsheet** — Claude reads `references/CHEATSHEET.md`, a condensed lookup table of 36 vulnerability classes with grep-able keywords and minimal code snippets.

2. **Codebase Sweep** — Two passes over the target Solidity code:
   - **Syntactic:** grep for trigger keywords from the cheatsheet
   - **Semantic:** read-through for logic bugs with no reliable grep signature (cross-function reentrancy, missing access control, etc.)

3. **Deep Validation** — For each candidate finding, Claude reads the full reference file (e.g., `references/reentrancy.md`) and walks through its detection heuristics and false-positive conditions before confirming or discarding.

4. **Report** — Confirmed findings are output with severity, code snippets, and fix recommendations.

## Project Structure

```
SKILL.md                        # Skill prompt (audit workflow + rules)
references/
  CHEATSHEET.md                 # Condensed quick-reference for all 36 vuln classes
  reentrancy.md                 # Full reference: preconditions, patterns, heuristics,
  overflow-underflow.md         #   false positives, remediation
  delegatecall-untrusted-callee.md
  ...                           # 36 reference files total
```

Each full reference file contains: **Preconditions**, **Vulnerable Pattern** (annotated Solidity), **Detection Heuristics**, **False Positives**, and **Remediation**.
