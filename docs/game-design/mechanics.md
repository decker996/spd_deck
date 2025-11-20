# Game Mechanics

**Game Title:** [Your Game Title]
**Last Updated:** [Date]

---

## Variables and Game State

### Primary Resources

Resources the player manages directly.

#### 1. [Resource Name]
- **Type:** [Integer / Float / Percentage]
- **Range:** [Min-Max, e.g., 0-10, 0-100%]
- **Starting Value:** [Initial amount]
- **Purpose:** [What does this represent?]
- **Sources:** [How to gain - actions, income, events]
- **Sinks:** [How it's spent - actions, costs, decay]
- **Critical Thresholds:**
  - < [value]: [What happens - e.g., "Crisis mode"]
  - > [value]: [What happens - e.g., "Abundance bonus"]

#### 2. [Resource Name]
[Repeat structure for each primary resource]

---

### Secondary Stats

Stats derived from or influenced by primary resources.

#### [Stat Name]
- **Type:** [Integer / Float / Percentage]
- **Range:** [Min-Max]
- **Calculation:** [How is it calculated?]
- **Purpose:** [What does this represent?]
- **Influences:** [What does this affect?]

---

### Flags and State Variables

Boolean flags or categorical variables tracking game state.

| Variable | Type | Purpose | Initial Value |
|----------|------|---------|---------------|
| `flag_name` | Boolean (0/1) | [Purpose] | 0 |
| `state_variable` | String/Enum | [Purpose] | "[Initial]" |

---

### Timers

Cooldown and tracking variables.

| Timer | Initial | Reset Value | Purpose |
|-------|---------|-------------|---------|
| `action_timer` | 0 | 6 | [Action] cooldown |
| `event_timer` | 0 | 12 | [Event] tracking |

---

## Time System

### Time Structure
- **Time Unit:** [Month / Turn / Day / Abstract]
- **Start:** [Start time, e.g., "January 1928"]
- **End:** [End time or condition]
- **Total Duration:** [Total units, e.g., "60 months"]

### Time Advancement
- **Method:** [Automatic / Player-controlled]
- **Trigger:** [What advances time? - e.g., "After each action"]
- **Actions per unit:** [How many decisions per time unit?]

### Time Pressure
- [ ] **Deadline-driven:** Must achieve goal by [date]
- [ ] **Event-driven:** Historical events on schedule
- [ ] **None:** Take as long as needed
- [ ] **Escalating:** Difficulty increases over time

### Calendar Events

| Time | Event | Type | Effect |
|------|-------|------|--------|
| [Date] | [Event name] | [Historical/Triggered] | [Description] |

---

## Core Mechanics

### 1. [Mechanic Name]

**Description:** [What is this mechanic?]

**How it works:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Player interaction:** [What does the player do?]

**Example:**
```
[Concrete example of mechanic in action]
```

**Implementation notes:**
```javascript
// Pseudocode or actual code
Q.variable += effect;
if (Q.condition) {
  // Do something
}
```

---

### 2. [Mechanic Name]
[Repeat for each major mechanic]

---

## Decision Structure

### Decision Types

#### Strategic Decisions
Long-term planning and positioning.

**Examples:**
- [Example 1 - e.g., "Choose economic policy direction"]
- [Example 2 - e.g., "Form alliance with faction"]

**Frequency:** [How often available?]
**Reversibility:** [Can be undone? Partial? Never?]

#### Tactical Decisions
Short-term optimization and resource management.

**Examples:**
- [Example 1 - e.g., "Spend 2 resources for +5 support"]
- [Example 2 - e.g., "Sacrifice short-term for long-term gain"]

**Frequency:** [How often available?]
**Reversibility:** [Can be undone? Partial? Never?]

#### Narrative Decisions
Character-driven, story-focused choices.

**Examples:**
- [Example 1 - e.g., "Trust advisor or investigate them"]
- [Example 2 - e.g., "Compromise values or stay pure"]

**Frequency:** [How often available?]
**Reversibility:** [Can be undone? Partial? Never?]

#### Reactive Decisions
Responses to events and crises.

**Examples:**
- [Example 1 - e.g., "Crisis! Bold action or caution?"]
- [Example 2 - e.g., "Opportunity! Seize it or pass?"]

**Frequency:** [How often available?]
**Reversibility:** [Can be undone? Partial? Never?]

---

## Card/Deck System

### Structure
- [ ] **Deck-based:** Cards drawn from pool
- [ ] **Always available:** All actions always accessible
- [ ] **Event-triggered:** Actions appear based on conditions
- [ ] **Hybrid:** Mix of above

### Card Categories

#### [Category 1 Name]
- **Quantity:** ~[number] cards
- **Availability:** [When available?]
- **Cooldowns:** [Typical cooldown period]
- **Examples:** [List 3-5 example cards]

#### [Category 2 Name]
[Repeat for each category]

### Draw Mechanics (if deck-based)
- **Draw size:** [How many cards shown at once?]
- **Frequency weighting:** [0-1000 system or other?]
- **Conditions:** [Can cards be conditionally unavailable?]
- **Pinning:** [Can some cards be always visible?]

---

## Progression System

### Power Progression
**Do players get stronger over time?**

**Unlocks:**
- [Unlock 1 - when and what]
- [Unlock 2 - when and what]

**Upgrades:**
- [Upgrade 1 - how acquired and effect]
- [Upgrade 2 - how acquired and effect]

### Challenge Escalation
**Does the game get harder over time?**

**Escalation mechanics:**
- [Mechanic 1 - e.g., "Opposition grows each year"]
- [Mechanic 2 - e.g., "Resource scarcity increases"]

### Unlock Progression
**What becomes available over time?**

| Unlock | Trigger | Effect |
|--------|---------|--------|
| [Name] | [Condition] | [What it enables] |

---

## Special Systems

### [System Name 1]

**Purpose:** [What is this system for?]

**Components:**
- [Component 1]
- [Component 2]

**Mechanics:**
[Detailed description of how this system works]

**Implementation:**
```javascript
// Key code snippets
```

---

### [System Name 2]
[Repeat for each special system - factions, relationships, economy, combat, etc.]

---

## Faction System (if applicable)

### Factions

#### [Faction 1 Name]
- **Identity:** [Who are they?]
- **Goals:** [What do they want?]
- **Starting strength:** [X%]
- **Starting dissent:** [Y%]
- **Satisfied by:** [What makes them happy?]
- **Angered by:** [What upsets them?]

#### [Faction 2 Name]
[Repeat for each faction]

### Faction Mechanics

**Strength:** [How is strength calculated/used?]

**Dissent:** [How does dissent affect the game?]

**Aggregate dissent calculation:**
```javascript
// Example calculation
Q.total_dissent = (
  Q.faction1_strength * Q.faction1_dissent +
  Q.faction2_strength * Q.faction2_dissent
) / 100;
```

---

## Relationship System (if applicable)

### Relationships

Tracked relationships with other entities (parties, characters, nations).

| Entity | Starting Value | Scale | Thresholds |
|--------|----------------|-------|------------|
| [Entity 1] | [50] | [0-100] | [Hostile <20, Neutral 40-60, Friendly >75] |

### Relationship Effects

**Mechanical effects:**
- [Effect 1 - e.g., "High relationship enables coalition"]
- [Effect 2 - e.g., "Low relationship blocks certain actions"]

**Narrative effects:**
- [Effect 1 - e.g., "Dialog changes based on relationship"]
- [Effect 2 - e.g., "Different scenes available"]

---

## Economy System (if applicable)

### Economic Variables
- **Budget:** [Description and range]
- **Inflation:** [Description and range]
- **Growth:** [Description and range]
- **Unemployment:** [Description and range]

### Economic Interactions

**How economy affects gameplay:**
- [Interaction 1]
- [Interaction 2]

**How player affects economy:**
- [Action 1 - effect on economy]
- [Action 2 - effect on economy]

---

## Balance Guidelines

### Resource Balance
- **Starting resources:** [Amount] (enough for [X] actions)
- **Monthly income:** [Amount] (~[Y]% of starting)
- **Major action cost:** [Amount] (~[Z]% of typical reserves)
- **Recovery time from mistake:** [N] turns

### Effect Scaling
- **Minor effect:** ±[5] ([10]%)
- **Moderate effect:** ±[10-20] ([20-40]%)
- **Major effect:** ±[30+] ([50+]%)

### Cooldown Lengths
- **Minor actions:** [3-6] turns
- **Major actions:** [6-12] turns
- **Unique events:** `max-visits: 1`

---

## Win/Loss Conditions (Detailed)

### Victory Conditions

#### Primary Victory: [Name]
- **Condition:** [Specific trigger]
- **Checked:** [When is this checked?]
- **Difficulty:** [Easy / Medium / Hard / Very Hard]

#### Alternate Victory: [Name]
- **Condition:** [Specific trigger]
- **Checked:** [When is this checked?]
- **Difficulty:** [Easy / Medium / Hard / Very Hard]

### Defeat Conditions

#### Hard Failure: [Name]
- **Trigger:** [What causes instant game over]
- **Preventable?** [Yes/No - How?]

#### Soft Failure: [Name]
- **Mechanism:** [How gradual decline works]
- **Recovery:** [Can player bounce back? How hard?]

---

## Advanced Mechanics

### Cascading Effects
[How do effects chain? Examples of major cascades]

### Emergent Gameplay
[What unexpected strategies might emerge?]

### Skill vs. Luck Balance
- **Skill:** [X]% - [Player decisions determine outcome]
- **Luck:** [Y]% - [Random events, dice rolls]

---

## Technical Implementation Notes

### Variable Initialization
```javascript
// In root.scene.dry
Q.variable1 = starting_value;
Q.variable2 = starting_value;
```

### Main Loop Structure
```
main.scene.dry → player choice → post_processing.scene.dry → main.scene.dry
```

### Critical Code Snippets

**Timer Management:**
```javascript
// In post_processing
for (var timer of Q.timers) {
  if (Q[timer] > 0) {
    Q[timer] -= 1;
  }
}
```

**Bounds Checking:**
```javascript
Q.resource = Math.max(0, Math.min(10, Q.resource));
```

---

**See Also:**
- [concept.md](concept.md) - High-level game concept
- [narrative.md](narrative.md) - Story and characters
- [characters.md](characters.md) - Character details
