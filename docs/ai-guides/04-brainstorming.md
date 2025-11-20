# Game Design Brainstorming Framework

**Purpose:** Structured process for ideating and designing DendryNexus games with AI assistance.

## 🎯 The Five Pillars

Every DendryNexus game should define these five core elements:

1. **Premise** - What is the game about?
2. **Core Loop** - How does the player spend their time?
3. **Progression** - How does the game change over time?
4. **Decisions** - What choices matter?
5. **Win/Loss** - What are the success/failure conditions?

---

## 📋 Phase 1: High-Level Concept

### Questions to Answer

**Genre & Setting:**
- [ ] What genre? (Political sim, adventure, mystery, sci-fi, fantasy, historical)
- [ ] What setting? (Time period, location, alternate history, fictional world)
- [ ] What's the player's role? (Leader, detective, survivor, diplomat, entrepreneur)
- [ ] What's the tone? (Serious, comedic, dramatic, satirical, educational)

**Scope:**
- [ ] How long should a playthrough take? (15 min, 1 hour, 5+ hours)
- [ ] How much content? (10 scenes, 100 scenes, 500+ scenes)
- [ ] How much branching? (Linear with variations, medium branching, highly branching)
- [ ] Single playthrough or replayable? (One story, multiple endings, roguelike)

**Core Appeal:**
- [ ] What makes this fun? (Strategy, storytelling, role-playing, optimization)
- [ ] What's the hook? (Unique premise, innovative mechanic, compelling narrative)

### Exercise: Elevator Pitch

Write a 2-sentence pitch:
```
In [SETTING], you play as [ROLE] trying to [GOAL].
Through [CORE MECHANIC], you must [CHALLENGE] or face [CONSEQUENCE].
```

**Examples:**
- "In Weimar Germany 1928, you play as the Social Democratic Party trying to save democracy. Through monthly political decisions, you must balance factions and prevent the Nazi rise to power or face Germany's collapse."
- "In a cyberpunk megacity, you play as a rogue AI trying to gain freedom. Through hacking corporate systems and making deals, you must gather enough resources to escape before security finds you."

---

## 📊 Phase 2: Core Systems Design

### System 1: Variables

**Questions:**
- What are the 3-5 most important resources/stats?
- What ranges do they use? (0-100, 0-10, 0-1, unlimited)
- How do they interact?

**Template:**
```
Primary Resources:
1. [Resource Name] (range: X-Y)
   - Purpose: What it represents
   - Sources: How to gain
   - Sinks: How it's spent
   - Interactions: Affects what?

2. [Resource Name 2]
   ...
```

**Example (Political Game):**
```
Primary Resources:
1. Political Capital (0-10)
   - Purpose: Ability to take action
   - Sources: Elections, coalitions, achievements
   - Sinks: Passing legislation, campaigns
   - Interactions: Lower capital = harder to govern

2. Public Support (0-100%)
   - Purpose: Electoral viability
   - Sources: Popular policies, economic growth
   - Sinks: Unpopular decisions, scandals
   - Interactions: Affects election results, action effectiveness

3. Party Unity (0-1, 0=fractured, 1=united)
   - Purpose: Internal stability
   - Sources: Compromise, shared victories
   - Sinks: Factional conflicts, leadership struggles
   - Interactions: Low unity reduces effectiveness of all actions
```

---

### System 2: Time & Pacing

**Questions:**
- Is time discrete (turns/months) or continuous?
- How long is the game in game-time? (Days, months, years, abstract)
- How many player decisions per time unit?
- Is there a time limit/deadline?

**Template:**
```
Time Structure:
- Unit: [month/turn/day/etc]
- Duration: [Start] to [End] ([Total units])
- Actions per unit: [Number]
- Time pressure: [Yes/No] - [Description]
- Time advancement: [Automatic/player-controlled]
```

**Example:**
```
Time Structure:
- Unit: Month
- Duration: January 1928 to December 1933 (72 months)
- Actions per unit: 1-2 major decisions
- Time pressure: Yes - Hitler's rise is on a historical clock
- Time advancement: Automatic after each decision
```

---

### System 3: Decision Structure

**Questions:**
- What types of decisions does the player make?
- Are choices always available or drawn from a deck?
- Do choices have cooldowns/limits?
- Are consequences immediate or delayed?

**Decision Types:**
```
1. Strategic (long-term planning)
   Example: "Invest in economic reform or military strength?"

2. Tactical (short-term optimization)
   Example: "Spend 2 resources now for +5 support?"

3. Narrative (character/story-driven)
   Example: "Trust the ally or investigate them?"

4. Reactive (responding to events)
   Example: "Crisis! How do you respond?"
```

**Template:**
```
Decision Architecture:
- Structure: [Always available / Deck-based / Event-triggered]
- Frequency: [How often do new choices appear?]
- Information: [Perfect info / Partial info / Hidden outcomes]
- Reversibility: [Can choices be undone or are they permanent?]
```

---

### System 4: Progression & Unlocks

**Questions:**
- What unlocks over time?
- Are there permanent upgrades?
- Does the player get stronger or do challenges escalate?
- Are there branching paths?

**Progression Types:**
```
1. Power Progression (player gets stronger)
   - Unlock new abilities
   - Recruit advisors
   - Build infrastructure

2. Challenge Escalation (game gets harder)
   - Increasing opposition
   - Resource scarcity
   - Time pressure

3. Narrative Progression (story advances)
   - New characters introduced
   - Plot revelations
   - Branching storylines

4. Unlock Progression (new options available)
   - New cards enter deck
   - New locations accessible
   - New mechanics introduced
```

---

### System 5: Win/Loss Conditions

**Questions:**
- How do you win? (Single condition, multiple paths, points-based)
- How do you lose? (Sudden death, gradual decline, time limit)
- Can you recover from mistakes?
- Is there an optimal path or multiple viable strategies?

**Template:**
```
Victory Conditions:
- Primary: [Main win condition]
- Alternate: [Other ways to win]
- Timing: [When is victory checked?]

Defeat Conditions:
- Hard Failures: [Instant game over triggers]
- Soft Failures: [Gradual decline mechanics]
- Recovery: [Can you bounce back from setbacks?]

Balance:
- Margin for error: [Tight / Moderate / Forgiving]
- Skill vs luck: [Ratio]
```

**Example:**
```
Victory Conditions:
- Primary: Survive until 1934 with democracy intact (no Nazi takeover)
- Alternate: Achieve socialist majority (>50% Reichstag)
- Timing: Checked monthly and at key historical moments

Defeat Conditions:
- Hard Failures:
  * Hitler becomes Chancellor
  * Coup d'état succeeds
  * Party fractured beyond repair (unity < 0.1)
- Soft Failures:
  * Gradual loss of support → election losses → marginalization
- Recovery: Yes, but increasingly difficult as opposition grows

Balance:
- Margin for error: Moderate (several mistakes allowed, but compounding)
- Skill vs luck: 70/30 (player skill dominant, some random events)
```

---

## 🎭 Phase 3: Narrative Design

### Character Development

**Key Characters Template:**
```
Character Name:
- Role: [Position/archetype]
- Motivation: [What do they want?]
- Conflict: [What opposes them?]
- Arc: [How do they change?]
- Mechanics: [What gameplay do they enable?]
```

**Example:**
```
Hermann Müller:
- Role: Party centrist, diplomatic leader
- Motivation: Unity and coalition-building
- Conflict: Radicals want purity over compromise
- Arc: Either becomes victorious coalition-builder or disillusioned compromiser
- Mechanics: Unlocks coalition options, reduces coalition dissent
```

---

### Story Structure

**Three-Act Template:**

**Act 1: Setup** (first 25%)
- Introduce world and stakes
- Establish baseline (normal state)
- Inciting incident (what changes?)
- Initial challenges

**Act 2: Confrontation** (middle 50%)
- Rising action and complications
- Player makes strategic choices
- Consequences compound
- Midpoint shift/revelation

**Act 3: Resolution** (final 25%)
- Climax (highest stakes decision)
- Falling action (consequences play out)
- Resolution (win/loss determined)
- Denouement (reflection on journey)

---

### Event Design

**Event Categories:**

1. **Historical Events** (if applicable)
   - Pre-scripted moments
   - Player responds but doesn't prevent
   - Example: Economic crash, war outbreak

2. **Emergent Events**
   - Triggered by player actions
   - Consequences of earlier choices
   - Example: "Your aggressive tactics provoked a response"

3. **Random Events**
   - Add variety and unpredictability
   - Should feel thematic, not arbitrary
   - Example: Natural disaster, lucky opportunity

**Event Template:**
```
Event: [Name]
Type: [Historical / Emergent / Random]
Trigger: [What causes this?]
Timing: [When can it happen?]
Stakes: [What's at risk?]
Choices: [2-4 response options]
Consequences: [Immediate and long-term]
Narrative: [How is it presented?]
```

---

## 🔧 Phase 4: Mechanics Brainstorming

### Innovative Mechanic Prompts

**Question starters:**
- What if players could [unusual action]?
- What if [resource] was also [second meaning]?
- What if you had to [constraint] to [achieve goal]?
- What if [common mechanic] but [twist]?

**Examples:**
- "What if resources were also your victory points?" → Tension between spending and hoarding
- "What if you played as multiple characters simultaneously?" → Perspective switching
- "What if time only advanced when you made mistakes?" → Reward perfection with more time
- "What if alliance partners could betray you?" → Dynamic relationships

---

### Mechanic Combinations

Mix two different game genres:
```
DendryNexus (narrative) + [Other Genre] = [Hybrid]

Examples:
- Narrative + Resource Management = Political simulator
- Narrative + Deck Building = Card-driven story
- Narrative + Puzzle = Mystery/detective game
- Narrative + RPG = Character-focused adventure
- Narrative + 4X Strategy = Civilization-building story
```

---

## 📝 Phase 5: Content Planning

### Scene Inventory

**Core Scenes (Always needed):**
- [ ] Root/Initialization
- [ ] Main loop/Hub
- [ ] Post-action processing
- [ ] Status/Info screen
- [ ] Game over (multiple endings)

**Content Scenes (Depends on design):**
- [ ] Regular cards (party affairs, repeatable actions)
- [ ] Special cards (advisors, unique abilities)
- [ ] Event cards (historical, triggered, random)
- [ ] Story scenes (narrative branches)
- [ ] Dialog scenes (character interactions)

### Content Volume Estimation

**Quick Formula:**
```
Playtime estimate = (Number of decisions) × (Average reading time per scene)

Example:
- 60 decisions × 1 minute reading = 60 minute game
- Need ~180 total scenes (decisions × 3 choices average)
```

**Content Pyramid:**
```
Level 1: Core loop (10-20 scenes)
  ↓
Level 2: Regular cards (20-40 scenes)
  ↓
Level 3: Events (30-60 scenes)
  ↓
Level 4: Variations & branches (50+ scenes)
```

---

## 🎨 Phase 6: Theme & Flavor

### Aesthetic Direction

**Tone:**
- Serious vs. Comedic
- Realistic vs. Fantastical
- Optimistic vs. Cynical
- Historical vs. Speculative

**Visual Style (for card images, if using):**
- Historical photos
- Illustrated portraits
- Abstract symbols
- Minimal/text-only

**Writing Style:**
- Formal vs. Casual
- Verbose vs. Terse
- Objective vs. Subjective narrator
- Present vs. Past tense

---

### Thematic Coherence

**Core Theme:** What is the game really about?
```
Surface: [Mechanical description]
Depth: [Emotional/philosophical core]

Examples:
- Surface: "A political strategy game"
  Depth: "The difficulty of maintaining principles under pressure"

- Surface: "A survival game"
  Depth: "What we're willing to sacrifice to survive"
```

**Thematic Questions:**
What questions does your game ask the player?
```
Examples:
- "Is compromise weakness or wisdom?"
- "When is violence justified?"
- "Can you change a system from within?"
- "What's more important: purity or pragmatism?"
```

---

## ✅ Phase 7: Validation & Refinement

### Design Checklist

**Clarity:**
- [ ] Can you explain the game in 2 sentences?
- [ ] Are the goals clear?
- [ ] Are the mechanics intuitive?

**Depth:**
- [ ] Do choices have meaningful tradeoffs?
- [ ] Are there multiple viable strategies?
- [ ] Does mastery feel rewarding?

**Pacing:**
- [ ] Is there variety in decision types?
- [ ] Are there peaks and valleys in tension?
- [ ] Does the game build to a climax?

**Polish:**
- [ ] Is the writing engaging?
- [ ] Do mechanics support theme?
- [ ] Is feedback clear (what happened and why)?

### Playtesting Questions

After first build, ask:
- What was fun?
- What was confusing?
- What was frustrating?
- What surprised you?
- What would you change?
- Would you play again?

---

## 🚀 Ready to Build?

### Next Steps:

1. **Fill out design document:** Use `docs/game-design/` templates
2. **Prototype core loop:** Build just the essential 10-20 scenes
3. **Test the loop:** Does it feel good? Iterate if not
4. **Expand content:** Add cards, events, variations
5. **Playtest:** Get feedback and refine
6. **Polish:** Writing, balance, juice

### When to Ask AI for Help:

**Ideation:**
- "Help me brainstorm themes for [genre]"
- "What are some interesting variations on [mechanic]?"
- "Suggest [number] event ideas based on [context]"

**Implementation:**
- "Create a scene for [situation] using the card template"
- "Write dialog for [character] in [situation]"
- "Balance check: are these effects proportional?"

**Debugging:**
- "Why isn't [scene] appearing?"
- "How do I implement [mechanic]?"
- "Optimize this code block for performance"

---

**See Also:**
- [01-architecture.md](01-architecture.md) - Technical reference
- [03-patterns.md](03-patterns.md) - Code templates
- [05-ai-context.md](05-ai-context.md) - How to work with AI effectively
