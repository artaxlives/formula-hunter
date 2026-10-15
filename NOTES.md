## Changed from the plan

No architecture or scope changes: the artifact is a single `index.html` with inline CSS, JavaScript, data generation, and SVG, with no build step or dependencies. Four reproducible datasets contain 64 observations each. A population of 240 random expression trees evolves using tournament selection, subtree crossover, point mutation, random immigrants, and elitism.

The plan describes an error-threshold solve as a string comparison. These measure different things: equivalent expressions can have different strings, and noisy observations cannot prove exact symbolic recovery. The reveal therefore reports the real hidden formula upon reaching the preset MSE threshold and separately compares the candidate and hidden expression strings, explicitly identifying an exact match or a differing expression. The acceptance threshold behavior is preserved.

## Could not do

None. The Node DOM harness exercises initialization, animation updates, pause/resume, all four dataset resets, leaderboard diversity, monotonic best-so-far error, and solved reveals. Static content remains meaningful before JavaScript runs.

## Omitted for lack of a service

None. All computation and visuals run locally; no service, account, key, or image generation is needed.
