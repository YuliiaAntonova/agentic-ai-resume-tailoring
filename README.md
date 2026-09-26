# Agentic AI ATS Resume Optimizer

An **Agentic AI workflow for tailoring resumes to job descriptions with ATS-aware prompt engineering**.

The project demonstrates how an AI system can analyze a job description, compare it with an existing resume, generate an optimized version, reflect on the result from a recruiter perspective, and produce an improved second version.

The workflow follows an agentic pattern:

```text
Analyze → Generate → Evaluate → Reflect → Regenerate → Compare
```

The goal is not simply to rewrite a resume. The system tries to improve alignment with a target vacancy while preserving the candidate's original experience and avoiding unsupported or fabricated skills.

---

## Project Overview

Many companies use **Applicant Tracking Systems (ATS)** to process resumes before they are reviewed by recruiters.

ATS systems may evaluate resumes based on factors such as:

- relevant keywords
- required skills
- technologies
- job-specific terminology
- experience alignment
- education requirements
- certifications
- resume structure

This project uses an **Agentic AI workflow** to analyze these elements and tailor a resume to a specific job description.

The system receives two main inputs:

```python
resume_text: str
vacancy_text: str
```

and produces:

```text
ATS Analysis
↓
Resume V1
↓
ATS Metrics V1
↓
Recruiter Reflection
↓
Resume V2
↓
ATS Metrics V2
↓
Visual Comparison
```

---

# Agentic AI Workflow

The project follows a multi-step AI workflow instead of using one large prompt.

```text
Original Resume
       +
Job Description
       ↓
ATS Analysis Agent
       ↓
Resume Generation Agent
       ↓
Resume V1
       ↓
ATS Metrics
       ↓
Recruiter Reflection Agent
       ↓
Resume V2
       ↓
ATS Metrics
       ↓
Visual Comparison
```

This architecture allows the system to evaluate its own first result and improve it in a second generation step.

---

# Main Workflow

The main pipeline is implemented through the following functions:

```python
analyze_ats_match()

generate_resume()

calculate_ats_metrics()

reflect_on_resume_and_regenerate()

generate_ats_charts()

run_workflow()
```

The complete workflow is orchestrated by:

```python
run_workflow()
```

---

# Step 1 — ATS Analysis

The first agent analyzes the job description and compares it with the original resume.

```python
analyze_ats_match(
    resume_text,
    vacancy_text,
    model
)
```

The analysis focuses on several resume optimization strategies.

The agent identifies:

- target role
- important ATS keywords
- required skills
- preferred skills
- tools and technologies
- soft skills
- experience requirements
- education requirements
- matching skills
- partially matching skills
- missing skills
- supporting evidence from the resume
- recommended areas to emphasize

The result is returned as structured JSON.

Example:

```json
{
  "target_role": "Data Scientist",
  "top_keywords": [
    "Python",
    "SQL",
    "Machine Learning",
    "AWS",
    "Docker"
  ],
  "matched_keywords": [
    {
      "keyword": "Python",
      "status": "matched",
      "evidence": "Developed Python-based data pipelines"
    }
  ],
  "missing_keywords": [
    {
      "keyword": "Kubernetes",
      "status": "missing",
      "evidence": null
    }
  ]
}
```

---

# Prompt Engineering Strategy

The project incorporates several resume prompt patterns.

These patterns are explicitly included in the Agentic AI workflow.

---

## 1. ACTION VERBS

The system identifies strong action verbs appropriate for the target role.

Example concept:

```text
What are some strong active verbs for this job title?
```

The system uses action verbs when rewriting resume bullet points.

Examples may include:

```text
Developed
Implemented
Designed
Automated
Optimized
Led
Built
Analyzed
Improved
Delivered
```

However, action verbs must describe experience already present in the original resume.

The agent is not allowed to create new responsibilities.

---

## 2. KEYWORDS

The ATS analysis identifies important keywords from the job description.

Concept:

```text
Analyze this job description and identify the most important
keywords that an ATS may track.
```

Possible keyword categories include:

```text
Programming Languages
Cloud Platforms
Frameworks
Databases
Methodologies
Technical Skills
Business Skills
Certifications
Soft Skills
```

Each keyword is compared with evidence from the candidate's original resume.

---

## 3. ROLE PLAY — Career Advisor

The first analysis agent also acts as a career advisor.

The agent identifies the most important aspects of the candidate's background that should be emphasized for the target role.

Concept:

```text
When crafting a resume for this position,
what should a career advisor recommend emphasizing?
```

This helps prioritize relevant experience instead of treating every resume section equally.

---

## 4. ROLE PLAY — Employer / Recruiter

The reflection agent evaluates Resume V1 from the perspective of a recruiter.

Concept:

```text
Act as a recruiter hiring for this role.

Review the candidate's resume against the job description.

Identify strengths, gaps, missing opportunities,
weak bullet points and unsupported claims.
```

This creates the reflection step in the agentic workflow.

---

## 5. REVISE

The system revises weak resume bullet points.

The general pattern is:

```text
Action + Task + Method / Skill + Result
```

For example:

```text
Before:

Worked with Python for reporting.

After:

Automated recurring reporting workflows using Python,
reducing manual data preparation and improving reporting consistency.
```

The optimized bullet can only contain information supported by the original resume.

---

## 6. SKILL COMPARISON

The system compares job requirements with candidate experience.

Each important skill can receive one of three statuses:

```text
MATCHED
PARTIAL
MISSING
```

Example:

```text
Python              MATCHED
SQL                 MATCHED
Machine Learning    MATCHED
AWS                 PARTIAL
Kubernetes          MISSING
Terraform           MISSING
```

This distinction is important because a missing skill should not automatically be inserted into the resume.

---

## 7. SKILL MATCH

The system determines which existing candidate skills should receive more visibility.

For example, if the job description repeatedly mentions:

```text
Python
SQL
Machine Learning
AWS
```

and those skills are already supported by the candidate's experience, the generated resume can highlight them more clearly.

The system does not simply insert every vacancy keyword.

---

# Evidence-Based Resume Optimization

A core design principle of this project is **evidence-based optimization**.

For each ATS
