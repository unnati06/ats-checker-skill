# ATS Resume Scorer — Claude Skill

A Claude skill that simulates an ATS (Applicant Tracking System) scan on your resume.
Paste a job description and your resume — get a match score, keyword gaps, section
ratings, and specific rewrite suggestions in seconds.

## What it does

Given a job description and a resume, this skill produces:

- **ATS match score** (0–100) with a strength label (Weak / Moderate / Strong / Excellent)
- **Keyword breakdown** — found, partial matches, and missing keywords
- **Section-by-section ratings** — Summary, Skills, Experience, Education
- **Top 3 priority fixes** — highest-impact changes ranked for you
- **Actionable suggestions** — specific, not generic ("add 'dbt' to your Skills section", not "improve your resume")

---

## Installation

1. Download [`ats-resume-scorer.skill`](./ats-resume-scorer.skill)
2. In Claude.ai → **Settings** → **Skills** → **Install skill** → upload the file
3. Done. The skill activates automatically on relevant prompts.

---

## How to use it

Once installed, just talk to Claude naturally:

```
"Score my resume against this job description: [paste JD] ... [paste resume]"
```

```
"What keywords am I missing for this role? [paste JD + resume]"
```

```
"Will my resume pass ATS for this job? [paste JD] Here's my resume: [paste]"
```

Claude will detect the intent and run the full analysis automatically.

---

## Example output

```
## ATS Score: 67/100 — Moderate

### Keyword match
Found (12): Python, SQL, Tableau, Git, Agile, REST APIs, data pipeline,
            stakeholder management, Excel, Scrum, JIRA, ETL

Partial (3): machine learning (resume says "ML models"), CI/CD (resume says
             "automated deployments"), communication (resume says "presented to teams")

Missing (7): TypeScript, dbt, BigQuery, A/B testing, WCAG, Next.js, OKRs

### Section strength
| Section    | Score | Notes                                          |
|------------|-------|------------------------------------------------|
| Summary    |  55%  | Doesn't mention the target role title          |
| Skills     |  80%  | Good coverage, missing dbt and BigQuery        |
| Experience |  70%  | Strong bullets but A/B testing absent          |
| Education  |  90%  | Meets all listed requirements                  |

### Top 3 priority fixes
1. Add "A/B testing" to your experience — it appears 4 times in the JD under Required
2. Update your summary to include "Data Analyst" and "fintech" — highest ATS weight section
3. Add dbt and BigQuery to your Skills section — both are in the Required qualifications
```

---

## Repo structure

```
ats-resume-scorer/
├── README.md
├── ats-resume-scorer.skill        ← install this in Claude.ai
└── skill/
    ├── SKILL.md                   ← raw skill instructions
    └── references/
        └── scoring-calibration.md ← scoring examples + synonym rules
```

---

## How it works

This skill is a `SKILL.md` file — a structured markdown document that teaches Claude
how to behave for a specific task. When you ask Claude to score a resume, Claude detects
the intent from the skill's description, loads the instruction file, and follows a
6-step process:

1. Collect the JD and resume
2. Extract keywords by category (hard skills, soft skills, qualifications, domain terms)
3. Score using a weighted match formula
4. Rate each resume section against the JD
5. Generate specific, prioritised suggestions
6. Format results with a priority fix list

The `references/scoring-calibration.md` file gives Claude real examples at different
score levels (88, 62, 31) to keep scores consistent across different roles.

---

## Extending this skill

Some ideas if you want to fork and improve it:

- **Bullet rewriter** — add a step that rewrites weak resume bullets using missing keywords
- **Multi-role comparison** — score one resume against several JDs and rank the best fit
- **Role presets** — bundle sample JDs for common roles (SDE, PM, Data Analyst) so users
  can test without a real JD
- **Export** — output results as a formatted PDF or markdown file

---

## Built with

- [Claude.ai](https://claude.ai) — Skills feature
- Skill format based on Anthropic's `SKILL.md` specification

---

## License

MIT — free to use, modify, and distribute.
