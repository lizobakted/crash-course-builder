# crash-course-builder

A Claude skill that turns lecture slides, textbook chapters and past tests into a single self-contained HTML exam crash course.

## What it does

- Asks about the test format, calculations, and question style (practice questions or question with model answer) before building.
- Builds highlighted summaries limited to your scope list, with slide images only for diagrams.
- Adds written practice questions in the format of your past tests, with model answers.
- Checks scope coverage and scans the prose for AI writing patterns before delivery.

## Install

Copy the `crash-course-builder/` folder into your Claude skills directory, or upload `crash-course-builder/SKILL.md` as a skill in Claude settings.

## Layout

```
crash-course-builder/
  SKILL.md   the skill instructions
README.md
```
