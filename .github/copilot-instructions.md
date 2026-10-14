# Copilot instructions

## Build, test, and lint

There are no build, test, or lint commands configured. The app has no server or external dependencies; open `index.html` directly in a browser to run it. For a smoke test, complete all 10 questions, check correct and incorrect feedback and progress/score updates, view the results, and use Play again.

## Architecture

The entire application lives in the root `index.html`: semantic page markup, design and responsive styles in an inline `<style>` block, and quiz behavior in an inline `<script>` block. There is no module system or build step.

The script owns the question data and the quiz state (`questionIndex`, `score`, and `hasAnswered`). Its flow switches among the welcome, quiz, and results sections; `showQuestion()` renders each question from the data, `chooseAnswer()` locks and scores a response and updates progress/feedback, and `showResults()` selects the final reaction and score message. The restart handler resets state and starts a new round.

## Codebase conventions

- Keep this a self-contained, dependency-free `index.html`; there is no framework, package manager, or separate asset pipeline.
- Define quiz content as entries in the `questions` array, with `category`, `question`, `options`, zero-based `answer`, and explanatory `fact`. The result thresholds and reactions are in `showResults()`.
- Use the existing CSS custom properties in `:root` for shared colors, with light-mode overrides in `@media (prefers-color-scheme: light)`. Keep responsive behavior in the existing media query and honor reduced motion in both CSS and JavaScript.
- Preserve the current accessible interaction pattern: native buttons for answers/actions, visible keyboard focus, live regions for score and feedback, progress-bar ARIA values, and focus moved to the new question or results heading after transitions.
- Keep answer selection single-use: `hasAnswered` prevents repeat scoring, and the answer buttons are disabled after a selection. Update the score, feedback, and progress together when changing answer behavior.
