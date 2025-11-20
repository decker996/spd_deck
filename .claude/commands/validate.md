# Validate Game Content

You are helping the user validate their DendryNexus game content for correctness and best practices.

## Instructions

1. **Identify what to validate:**
   - Ask the user what they want validated (specific scene, entire game, balance check)
   - Get file paths or code snippets if needed

2. **Run comprehensive validation using checklist from `docs/ai-guides/05-ai-context.md`:**

### Scene Structure Check
- [ ] Has required properties (`title:`)
- [ ] Proper subscene format (`@subscene_id`)
- [ ] All `go-to:` targets exist
- [ ] No dead-end choices (unless intentional game over)

### Variable Check
- [ ] All variables used are initialized in `root.scene.dry`
- [ ] Variables use `Q.` namespace in code blocks
- [ ] No typos in variable names
- [ ] Consistent naming conventions

### Conditional Logic Check
- [ ] `view-if:` syntax correct
- [ ] `choose-if:` syntax correct
- [ ] String comparisons use quotes
- [ ] Boolean logic correct (`and`, `or`, `not`)

### Code Block Check
- [ ] JavaScript syntax valid
- [ ] Code properly wrapped in `{! ... !}`
- [ ] No unintended global variables (`var` without `Q.`)
- [ ] Bounds checking (no negative resources, percentages 0-100)
- [ ] No division by zero
- [ ] Arrays/objects initialized before use

### Card Mechanics Check (if applicable)
- [ ] `is-card: true` present
- [ ] `frequency:` set appropriately
- [ ] `view-if:` includes timer check for repeatable cards
- [ ] `on-arrival:` sets timer for cooldown
- [ ] Timer variable initialized in `root.scene.dry`
- [ ] Timer decremented in `post_month.scene.dry` or similar

### Balance Check
- [ ] Costs proportional to benefits
- [ ] Resources can recover from bad decisions
- [ ] No single dominant strategy
- [ ] Cooldowns appropriate for action power

### Narrative Check
- [ ] Choices have clear mechanical effects
- [ ] Subtitles preview consequences
- [ ] Text adapts to game state with conditionals
- [ ] Tone consistent with game
- [ ] No walls of text (readable paragraphs)

3. **Provide detailed report:**

```
## Validation Report: [Scene/File Name]

### ✅ Passed Checks
- [List what's correct]

### ⚠️ Warnings
- [List potential issues that aren't errors but could be improved]

### ❌ Errors
- [List actual errors that will break the game]

### 💡 Suggestions
- [List improvements for clarity, balance, or polish]

## Recommended Fixes

[Provide specific code changes for each error/warning]
```

4. **Offer to fix issues:**
   - "Would you like me to fix these errors?"
   - "I can help implement these suggestions if you'd like."

## Common Issues to Watch For

### Critical Errors
- Uninitialized variables
- Invalid JavaScript syntax
- Missing `go-to` targets
- Timer not set after card use
- Resources going negative

### Balance Issues
- Effects too strong/weak compared to similar actions
- No meaningful choice (one option clearly dominant)
- Impossible to recover from mistakes
- Exponential growth without bounds

### Polish Issues
- Missing flavor text
- No conditional narrative adaptation
- Unclear choice effects
- Missing edge case handling

## Reference Documentation

- Validation checklist: `docs/ai-guides/05-ai-context.md`
- Architecture reference: `docs/ai-guides/01-architecture.md`
- Pattern examples: `docs/ai-guides/03-patterns.md`

Start by asking: "What would you like me to validate?"
