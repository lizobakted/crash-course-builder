---
name: "crash-course-builder"
description: "Build a single-file HTML exam crash course from lecture slides, textbook chapters and past tests. Use for 'make a course/study guide/summary for my test', 'prepare me for exam', or any scope list of slides and chapters."
---

# Crash course builder

Turn a student's scope list, slide decks, book and sample tests into one self-contained HTML file they can study from and take notes on. The student is busy and the test format differs from course to course, so the course must be exact on scope, easy to scan, and practise the format of this particular test.

## Step 0: ask before building
Test formats vary a lot, and a wrong guess means rebuilding the whole question section. Before reading sources in depth, ask the student with AskUserQuestion (one call, up to four questions, skip any already answered in their message or visible in an attached sample test):
1. **Test format.** What does the test look like: written open questions, multiple choice, true/false, short answers, problem solving, a mix? Roughly how many points per question type? Encourage them to attach a past test or sample paper; if one is attached, read it and confirm your reading in one line instead of asking.
2. **Calculations.** Are there calculations or formulas on the test? Include a calculations section only if yes.
3. **Question style.** Do they want (a) **practice questions**: the question first, a place to think or answer, then a hidden model answer to check against and self-score, or (b) **just question and model answer**: both visible together for reading and memorising.
4. **Extras.** Anything they do not want (mock test, quiz, exercises) and whether slides with diagrams should be included as images.
If the student is not around or does not answer, pick the most likely reading (written questions, practice style with hidden answers, no calculations), say which in one line at the top of your work, and continue.

Use the answers to decide the question formats, the points labels, the answer length and whether a calculations section exists. Do not hard-code any one test's format.

## Inputs to pin down (from the message, not by asking)
- Scope list: which lecture slides (numbers) and which book chapters/sections, plus the "special attention" items.
- Source files: slide PDFs, book PDF, previous tests. Identify which file is a **test** (past paper or solution) and which is lecture material. Never treat a test as lecture content; use it only for question format and to see what gets asked.
- If the student corrects the scope later (removes a bullet, drops a section), delete it completely, including nav entries, questions and mentions.

## Workflow
1. Ask the Step 0 questions.
2. Extract text with pdftotext (keep layout) and render slides with pdftoppm. Slides often carry no printed numbers, so map the student's slide numbers to PDF page order and verify by reading the page.
3. Read the past tests. Note the question types and points and mirror them in the practice section (example from one course: 8-pt "describe four of six items", 5-pt "differences among three concepts", 2-pt "concept plus example"; yours may differ entirely).
4. Write sections only for scoped slides and book items. Book chapters may sit in different chapters than the user's labels (check, and say so briefly).
5. Build, render-check, scan for AI tells, then deliver.

## Content rules (learned from the student's corrections)
- **Prioritise lectures over the book.** Use the book only for items the scope names. When a slide and the book disagree, show the slide's version and drop the contradiction silently (do not write about it).
- **Slides: images only for diagrams.** If a slide just repeats the text below, keep the text and omit the slide. Crop to the diagram when the slide is mostly text.
- **Examples instead of commentary.** Give a concrete real-world example for definitions, lists and metrics. Do not add "my readings", "exam angle", "write in your notes" or "start here" boxes.
- No mock test, no quick quiz and no usage paragraph above the questions unless the student asks. Do not explain how to use the questions.
- Keep every listed slide point present. After building, run a term-by-term coverage check (list the key terms of each slide and grep the built HTML).
- Use the textbook's term where a slide uses another (add it in a column) only when it helps matching.

## Practice questions
- Formats come from Step 0 and the past tests. Tag each question with its source slide or book section.
- **Practice style:** question, then a collapsible model answer (details element), so the student answers first.
- **Question and model answer style:** show the answer directly under each question, no collapse.
- Lecture-sourced first; book-sourced only for the scoped book items.
- Model answers are exactly what earns points: the named items, one line each, plus an example where the format asks for one. Match the answer length to the points.
- For multiple choice, make exactly one option correct and explain why each distractor fails.

## HTML design
Single self-contained file: inline CSS and JS, base64 JPEG slide images (about 1000 px wide), inline SVG for diagrams you draw yourself. Build with a small Python script that concatenates part files and replaces image tokens so each section can be edited separately.
- Left sidebar nav (lectures, book chapters, practice) with a mobile menu button, progress checkboxes per section stored in localStorage inside try/catch, active-link highlighting with IntersectionObserver, a Highlights on/off toggle, light and dark theme via CSS variables, print CSS.
- Highlight system: yellow mark = term, green = list or formula, red = easy to mix up. Provide a small legend.
- Readability components: callout for a definition, cards in 2 to 4 column grids for parallel items, flow with arrows for processes, short tables (Term / Meaning / Example), spectrum bars for scales, figure with caption "Lecture N, slide M". Break any paragraph above about 330 characters into cards, a table or a list. Put examples on their own line.
- Tables and cards beat long bullets. One idea per bullet.

## Writing style (humanizer pass)
Run the humanizer skill over all prose before delivery. Specifically avoid: em and en dashes, curly quotes, not X but Y contrasts, one-line closers, forced triads, stock words (crucial, pivotal, robust, landscape, enhance, additionally), captions that describe the document itself. Keep slide wording intact inside quotes. Plain, direct sentences a student can copy into notes.

## Verification before delivery
- Rebuild and confirm no unresolved image tokens.
- Dump the DOM in headless Chrome to confirm all question cards and details elements render; screenshot new components (hash anchors do not scroll in headless, so extract the section into a temp HTML if needed).
- Scan for dashes, curly quotes and AI words; delete temp files.
- Deliver the HTML with SendUserFile as an attachment and give a two to three sentence summary: what is covered, anything resolved or dropped, and how many questions. Do not recap the steps.

## When the user asks for changes
Edit the relevant part file, rebuild, rerun the scans, resend. Typical requests: remove a section, make a section less text heavy (turn into cards, flow, table), add examples, replace a hand-drawn diagram with the actual slide, re-run humanizer, re-verify coverage.