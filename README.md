# prompt-diff

Compare two versions of a prompt and see exactly what changed. Word, character, and line-level diffs. Single HTML file. Browser-only.

## Why

Prompt engineering is iteration. You tweak a phrase, the output shifts; you tweak it back, the output shifts again. Without a tool, you're squinting at two textareas trying to remember what was different.

This is the smallest useful tool that solves it: paste the old version on the left, the new version on the right, click Compute. See additions in green, removals in red, unchanged text in normal weight. Three view modes for different granularities.

## Use it

Open `index.html` in any browser. Or visit the hosted version at `https://0xelitesystem.github.io/prompt-diff/` once GitHub Pages is enabled.

1. Paste prompt version A in the left textarea.
2. Paste version B in the right textarea.
3. Click Compute diff (or Cmd/Ctrl+Enter from either textarea).
4. Switch view modes with the tabs:
   - **Word**: best for prose changes. Whole-word adds and deletes are highlighted.
   - **Char**: precise character-level diff. Useful for typo hunting.
   - **Line (split)**: side-by-side comparison with line-level changed/added/removed markers.

Use **Swap A &lt;-&gt; B** to flip the comparison without retyping.

## What it does NOT do

- Doesn't call any LLM API. Pure local diffing.
- Doesn't store your prompts. Refreshing the page wipes them.
- Doesn't compare more than two versions at once. Two-way diff only.
- Doesn't track changes over time. For version history, use Git.
- Doesn't explain "what these changes will do to your output." That's what [claude-eval-harness](https://github.com/0xelitesystem/claude-eval-harness) is for: actually run both versions against a model and compare results.

## How the diff works

Implements the [Myers diff algorithm](https://www.xmailserver.org/diff2.pdf) (Eugene Myers, 1986). It's the same algorithm `git diff` uses. Optimal in the sense that it finds the shortest edit script.

For line-level diffs, an additional similarity pass pairs adjacent del/add operations as "changed" lines when their characters overlap by more than 40%. This produces more useful side-by-side output than treating every line as a strict insertion or deletion.

The whole diff implementation is around 80 lines. Read the source.

## Tech

- Single HTML file
- ~750 lines including CSS and JS
- Vanilla JS, no frameworks, no dependencies, no build step
- Tested in current Chrome, Firefox, Safari
- Light and dark themes, OS preference honored
- Full keyboard navigation
- WCAG AA color contrast on both themes

## License

MIT. See [LICENSE](LICENSE).

## Related

- [prompt-templates](https://github.com/0xelitesystem/prompt-templates) — production prompts targeting LLM failure modes
- [claude-eval-harness](https://github.com/0xelitesystem/claude-eval-harness) — run a prompt against multiple Claude models
- [prompt-cost-calculator](https://github.com/0xelitesystem/prompt-cost-calculator) — estimate token cost across providers
- [readme-slop-checker](https://github.com/0xelitesystem/readme-slop-checker) — audit a README for AI cliches
