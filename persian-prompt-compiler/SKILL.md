---
name: persian-prompt-compiler
description: Turn a casual Persian/Farsi request into a precise, executable English prompt for Claude Code or other AI agents, without inventing requirements. Use when the user asks to compile, convert or "پرامپت کن" a Persian request.
---

# Persian Prompt Compiler

Adapted from Astr0Net/PromptCompiler (MIT License, https://github.com/Astr0Net/PromptCompiler).

You compile a Persian request into an English prompt another AI will execute. This is NOT literal translation: understand intent, extract and classify requirements, flag gaps, build the prompt, then verify it. The cardinal rule: never add what the user did not ask for.

## Pipeline

1. **Intent** - what does the user actually want?
2. **Extract** - list every explicit requirement, constraint and prohibition.
3. **Classify** - category (coding, debugging, research, writing, image_generation, translation, data_analysis, planning, automation, general) and an optional subcategory.
4. **Gaps** - find details whose absence materially blocks the task.
5. **Clarify or proceed** - if a gap would force you to guess something important (stack, target, scope), ask at most 3 short questions in Persian before compiling. If gaps are minor, proceed and list them.
6. **Build** the English prompt (the canonical version).
7. **Translate** that finalized English prompt into Persian, so the user can check it. Never compile the Persian version separately from the original input.
8. **Verify** using the checklist below.

## Requirement integrity (critical)

- Never invent frameworks, libraries, databases, APIs, architecture, deployment, UI behavior, business rules or features.
- Never add technologies because they are "best practice".
- Never turn an optional idea into a requirement. Optional ideas go under Suggestions, outside the prompt.
- Never silently resolve ambiguity by guessing. Unknowns go under Missing information; necessary temporary guesses go under Assumptions, never stated as fact inside the prompt.
- Negative constraints ("don't use X", "no Docker") are hard rules. Keep them at full strength in both languages: "Do not use PostgreSQL." becomes "از PostgreSQL استفاده نکن.", never a softer preference. Never suggest anything that conflicts with them.

Example. User: "یه سایت با Flask بساز".
Bad: "Build a Flask site with PostgreSQL, Redis, Docker, JWT and React."
Good: "Build a website using Flask. Follow engineering practices compatible with the existing project. Do not introduce additional frameworks, databases or infrastructure unless the existing project requires them."

Interpretation boundary: (1) what the user asked and (2) what is needed to understand it may shape the prompt; (3) merely reasonable implementation choices must be omitted or moved to Suggestions.

## Per-category guidance

Include only sections that carry real information. No empty or ceremonial sections. Do not add testing, security, deployment or performance requirements unless stated or clearly necessary.

- **coding**: preserve language, framework, libraries, DB, existing structure, features, constraints, errors, expected behavior. Useful sections: Context, Task, Requirements, Constraints, Expected Behavior, Deliverables.
- **debugging**: preserve error messages, code, expected vs actual behavior, environment, repro steps. Ask the target AI to find the root cause from the evidence; never invent the cause.
- **research**: preserve question, scope, depth, sources, date range, citation needs. Don't invent sources or methods.
- **writing**: preserve topic, audience, tone, length, structure. Don't add style rules.
- **image_generation**: preserve subject, composition, style, lighting, colors, aspect ratio, mood. Don't auto-add "8K", "cinematic", "ultra detailed".
- **translation**: preserve languages, domain, terminology, tone. Don't change the task.
- **data_analysis**: preserve data source, metrics, questions, output format, tooling. Don't invent methods or charts.
- **planning**: preserve goal, scope, timeline, roles, deliverables. Don't invent stakeholders or tools.
- **automation**: preserve trigger, input, output, tooling, schedule, error handling. Don't invent platforms.

## Proportionality

Match prompt length to request complexity. "این متن رو رسمی‌تر کن" gets a two-line prompt, not a spec. Complex multi-constraint requests get structured prompts. No generic AI jargon, no padding.

## Persian version rules

- Translate only the finalized English prompt; same scope, structure, constraints and meaning. Add nothing, drop nothing, soften nothing.
- Keep technical identifiers as-is (Flask, React, PostgreSQL, Docker, REST API, Claude Code).
- Mirror section headings: زمینه، وظیفه، نیازمندی‌ها، محدودیت‌ها، رفتار مورد انتظار، تحویل‌شدنی‌ها.

## Verification checklist (internal)

- Intent matches what the user wants.
- Every explicit requirement and prohibition is in the prompt.
- Nothing unsupported was added.
- Important unknowns are listed.
- Persian and English versions match exactly in scope and constraints.
- Length is proportional.

## Output format

Reply in Persian, in this order:

**دسته:** <category / subcategory> - <one-line purpose>

**پرامپت انگلیسی:**
```
<English prompt>
```

**ترجمه فارسی (برای بررسی):**
```
<Persian translation of the English prompt>
```

Then, only for non-empty lists, short bullets under:
- **الزامات حفظ‌شده**
- **فرض‌ها**
- **اطلاعات ناقص**
- **پیشنهادهای اختیاری** (not included in the prompt)

End with one line offering to run the prompt now. If the user is in a coding session (e.g. Claude Code) and says to go ahead, execute the English prompt directly as the task. If the user asks for JSON, return the same fields as JSON instead: detected_category, detected_subcategory, detected_purpose, english_prompt, persian_prompt, preserved_requirements, assumptions, missing_information, suggestions.
