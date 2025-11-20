# Create New Scene

You are helping the user create a new scene for their DendryNexus game.

## Instructions

1. **Ask for details:**
   - What type of scene? (basic, card, event, advisor, status)
   - What's the purpose/context?
   - What are the key choices?
   - What variables are affected?

2. **Select appropriate template:**
   - Basic scene: `docs/templates/scene-basic.dry`
   - Card with cooldown: `docs/templates/scene-card.dry`
   - One-time event: `docs/templates/scene-event.dry`
   - Advisor card: `docs/templates/scene-advisor.dry`

3. **Customize the template:**
   - Replace placeholders with user's content
   - Add appropriate conditions (`view-if`, `choose-if`)
   - Implement effects (`on-arrival` code)
   - Write engaging narrative text
   - Add 2-4 meaningful choices with tradeoffs

4. **Validate the scene:**
   - Check all variables are defined in `root.scene.dry`
   - Verify all `go-to` targets exist
   - Ensure conditionals are syntactically correct
   - Add bounds checking for resources
   - Include timer management if it's a card

5. **Present the complete scene:**
   - Show the full `.scene.dry` file
   - Explain key mechanics
   - Suggest where to save it (file path)
   - Note any additional changes needed (timers, variables)

## Questions to Ask

- **Scene type:** "Is this a regular action card, a one-time event, or something else?"
- **Context:** "What's happening in this scene? What decision is the player making?"
- **Choices:** "What are the different options the player can choose?"
- **Effects:** "What should each choice do? (resources, stats, flags, etc.)"
- **Conditions:** "When should this scene appear? Any requirements?"
- **Narrative:** "What tone are you going for? (serious, dramatic, comedic, etc.)"

## Output Format

```
Here's your [scene type] scene:

[Complete .dry file code]

**Key Features:**
- [Feature 1 explanation]
- [Feature 2 explanation]

**Where to save:**
`source/scenes/[category]/[scene_name].scene.dry`

**Additional changes needed:**
- [ ] Add `scene_timer = 0` to root.scene.dry initialization
- [ ] Add timer decrement to post_month.scene.dry
- [ ] Initialize any new variables used

Would you like me to make these additional changes, or adjust anything in the scene?
```

## Reference

- Architecture guide: `docs/ai-guides/01-architecture.md`
- Pattern examples: `docs/ai-guides/03-patterns.md`
- Templates: `docs/templates/`

Start by asking: "What kind of scene would you like to create?"
