# Formula Hunter

A client-side genetic-programming engine that evolves mathematical expressions generation by generation until it rediscovers the hidden formula behind a noisy scatter plot, entirely in the browser.

**Live:** https://formula-hunter.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/thePandaily/status/2110226743797915911

## Why this one

SRHarness/MechBench's core idea — can a system recover the mechanism behind data, not just fit it — translates cleanly into a single page: pick a preset dataset (synthetic, generated from a hidden formula plus noise, authored into the artifact), hit run, and watch a real genetic-programming population of candidate expressions mutate, breed, and converge on the true formula live, with a fitness chart and the leading expression rendered as you go. Every number on screen is a live computation happening in the visitor's own browser; nothing is fetched, faked, or precomputed at build time, so it survives the no-outbound-network constraint without any asterisk. It beat AI Badger's codebase-map idea on the tiebreak: that idea is just as honest and demonstrable, but doing it properly wants real external repos analyzed for density/language stats, which under this sandbox's no-network build step means either vendoring someone else's repo snapshots (stale, and a provenance question) or settling for a toy repo that undercuts the 'map any codebase' claim — Formula Hunter needs no external data of any kind, since synthetic ground-truth formulas are the normal, honest way to demo symbolic regression. Everything else failed the single-web-artifact or no-paid-API filters outright before taste was even a factor.

## What it turned down

- **AI Badger 0.8.0 Codebase Map** — Closest runner-up on taste, but a faithful build needs real external repos analyzed for size/density, and under this sandbox's no-outbound-network build step that means vendoring stale snapshots or settling for one toy repo, which undercuts the 'map any codebase' claim that makes the original interesting.
- **Open-Science (aipoch_ai)** — It's a desktop app whose entire appeal is an agent reading papers and running approved code live — not a single static web artifact, and the interesting part depends on live LLM calls.
- **Menlo SDK** — Controls physical Asimov robot hardware over mic/camera/speaker — cannot exist as a deployable web artifact with no hardware behind it.
- **Catalyst (imbue_ai)** — The 'competing explanations survive to the next experiment' loop is a research workflow that needs real experiment data and iterative LLM judgment to mean anything — a static mockup of it would be a mockup pretending to work, which the honesty rule forbids.
- **vercel-labs/skills** — It's an npx/TypeScript packaging tool for agent skill packs — a developer CLI library, not something a stranger clicks through in a browser in 10 seconds.
- **CLI-Anything (HKUDS)** — The product is a CLI generator for wrapping GUI apps — a CLI by definition, explicitly out of scope.
- **pentagi (vxcontrol)** — A multi-agent penetration-testing orchestrator whose value is coordinating live attack chains against real targets — not a self-contained browser demo, and not something to reshape into one responsibly.

## How it works

Vanilla JS, inline CSS, one index.html, no build step — the whole project is small enough that a bundler would only add failure modes. Candidate formulas are expression trees over {+, -, *, /, sin, cos, pow2, x, constants}; fitness is negative mean-squared-error against a noisy dataset generated at load time by a seeded PRNG (mulberry32) so every reload is reproducible without fetching or precomputing anything externally. Each animation frame advances several generations (tournament selection, subtree crossover, point mutation) so visible convergence on the simple preset formulas happens within a few seconds, not minutes. All visuals are SVG and HTML, not canvas: data points are `<circle class='data-point'>`, the leading fit and population curves are `<path>` elements, and the leaderboard is a `<ul>` of `<li class='leaderboard-item'>` — this is what makes the run mechanically inspectable by a headless browser instead of just visually plausible. Four preset hidden formulas (e.g. quadratic, damped trig, rational, interaction term) ship inline as PRNG seeds plus the true expression, so the 'reveal' on solve is a real string comparison, not a canned animation.

## What it was held to

- before any interaction, the page contains at least 40 elements matching .data-point
- without any click, the #generation element's text changes from its initial value within 3 seconds of page load
- #leaderboard contains at least 8 elements matching .leaderboard-item, with at least 5 distinct text contents among them
- clicking #pause-btn freezes #generation's text for at least 2 seconds afterward, and the button's label changes to indicate paused state
- changing #preset-select to a different option resets #generation's text to "0" and replaces the .data-point elements with a new set of coordinates
- #best-fitness holds a numeric value that is less than or equal to its value from 1 second earlier at every sampled point after generation 1 (best-so-far fitness never regresses)
- once #best-fitness drops below the active preset's solved threshold, #solved-banner loses its hidden attribute and its text contains an "=" character

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
