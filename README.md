# testwright

AI-assisted test case generation — structured, traceable, CI/CD-ready.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

-----

## What it does

Give it requirements. Get back structured, traceable test artifacts.

|Artifact           |Default format    |Alternatives                 |
|-------------------|------------------|-----------------------------|
|Test scenarios     |Gherkin `.feature`|Plain Given/When/Then        |
|Test case templates|JUnit XML         |Xray, Zephyr, TestRail, qTest|
|Coverage matrix    |XML               |JSON, Markdown table         |

Every artifact includes `humanReviewed=false` and full traceability:
Requirement → Test Condition → Test Case → Scenario → Template → Coverage.
Every response ends with a **Human Validation Required** checklist.

-----

## Quick start

**Option A — Claude, GPT, Gemini (system prompt)**

1. Open [`SKILL.md`](SKILL.md) and copy the contents
1. Paste as system prompt or project instructions
1. Send your requirements in the chat

**Option B — Claude Skill**

Import [`SKILL.md`](SKILL.md) into your Claude skill library.
testwright triggers automatically when requirements and testing are involved.

See [`examples/`](examples/) for sample input and output.

-----

## What goes in

Anything requirement-shaped:

- Free-text requirements (`The system MUST …`)
- User stories (`As a … I want … so that …`)
- Acceptance criteria
- ReqIF excerpts

-----

## What comes out

**Section A — Test Scenarios** (Gherkin by default)

```gherkin
@reqId:REQ-001 @priority:HIGH @testType:BVA @confidence:HIGH
@derivedFrom:BVA @ecRef:EC-V-REQ001-001
Scenario Outline: TC-REQ001-BVA-001 Password length boundary (<description>)
  ...
```

**Section B — Test Case Templates** (JUnit XML by default)

```xml
<testcase name="TC-REQ001-BVA-001_PasswordLength8Chars">
  <properties>
    <property name="oracle" value="HTTP = 200 AND password_hash updated"/>
    <property name="humanReviewed" value="false"/>
  </properties>
</testcase>
```

**Section C — Coverage Matrix**

```xml
<entry requirementId="REQ-001">
  <coverageStatus>COVERED</coverageStatus>
  <summary coveragePercent="100.0"/>
</entry>
```

-----

## Test design techniques

Applied automatically based on what the requirements contain:

|Technique               |Applied when                                      |
|------------------------|--------------------------------------------------|
|Equivalence Partitioning|Always                                            |
|Boundary Value Analysis |Numeric, string length, date, enum, boolean inputs|
|Decision Tables         |≥2 conditions jointly determine outcome           |
|State Transitions       |≥2 distinct states identified                     |

-----

## TC-ID scheme

```
TC-[REQ-ID]-[TYPE]-[NNN]

POS   Positive / Happy Path       NEG   Negative / Error path
BVA   Boundary Value Analysis     EDGE  Edge case
STA   State Transition            DT    Decision Table
```

-----

## Files

```
testwright/
├── SKILL.md          ← prompt + skill definition (one file)
├── examples/
│   ├── requirements_login.txt
│   └── output_login/
│       ├── login.feature
│       ├── TEST-LoginSystem.xml
│       └── coverage_matrix.xml
├── LICENSE
└── README.md
```

-----

## Tips

- Works best with `max_tokens` ≥ 8192
- For large requirement sets (>10 REQs): provide in batches by feature area
- Mention your toolchain (Xray, Zephyr, TestRail…) for adapted output format
- After review: set `humanReviewed=true` in your test management tool

-----

## License

MIT — see <LICENSE>.