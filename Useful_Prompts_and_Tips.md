# AI Coding Prompts

If having an issue that the agent says it has fixed but hasn't try something like:

`That didn't fix it. Please first reproduce the problem, prove you've reproduced it, find the root cause, fix it and prove you've fixed it.`

Add test coverage before making changes or upgrading an app:

`Review the entire project and ensure it has good test coverage`

Then:

`Very good. Now check for any out of date dependencies, secuirity issues and vulnerabilities then fix them and validate all tests still pass after remediating all secuirty issues`

Code review:

`Please carry out a comprehensive code review of the entire repo, and write a report with actions to code_review.md in the docs folder`

Or secuirty code review:

`Please carry out a comprehensive secuirty code review of the entire repo, and write a report with actions to code_review.md in the docs folder prioritizing by criticallity`

Then:

`OK thank you, please go ahead and address all the High, Medium and Low priority issues and retest everything and let me know when everything is remediated and tests ok`

# Debugging Strategy

- **Snapshot** - do a git commit
- **Paste the trace** - start with this to let it figure out & if unable to or makes it worse revert to snapshot & do more regimented approach below
- **Guide with debug.md** - Guide it through, giving it these instructions
	1. **Reproduce consistently:** ask it to reproduce the issue & document it, e.g. in debug.md.
	2. **Investigate, hypothesis:** tell it to look at all the related logs, put in extra logging info, get all the info you can, investigate deeply, come up with hypothesis for what could be causing this, & document them in debug.md or whatever.md.
		- Include in that doing a web search for other people that have has these problems.
		- Validate any reported GitHub or Stack Overflow issues it says matches the problem it's found, especially if it says "this is a known problem" - red flag! - isn't a one-off reported by one person many years ago that can't be the same issue.
		- Sanity check: just because you found one other recorded instance of this issue: do you have evidence that this is a common issue? Did more than one person report it? Does it really seem like this is the same issue we're encountering?
		- Challenge it hard, don't jump to conclusions.
	3. **Demonstrate root cause:** when it comes up with a root cause it must demonstrate it, prove that is really the root cause, and document the proof in debug.md.
	4. **Fix and prove:** now you know the root cause fix it & prove that your fix consistently fixes the issue.
	5. **Lessons learned in CLAUDE.md:** document lessons learned in CLAUDE.md. Come back and recognize what you did, learn from it for this project, put down some tips so that you don't repeat the same mistake again.

It is common to find it going off the rails and going on a tangent that proves to be a red herring. If this happens, go back to your git commit, wipe everything out, put a note about what the problem is not, and then start over again.

**Pro tip:** using a different agent, a different LLM and give it the assignment instead. Make it go through this process, because it's like having a second pair of eyes.

Or consider a Skill like this one: [systematic-debugging](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging)