# DendryNexus Architecture Guide

**Purpose:** Technical reference for understanding DendryNexus framework internals.

## 📁 File Format: `.dry` Files

DendryNexus uses `.dry` files (DendryScript) - a markdown-enhanced format for interactive narrative.

### File Types

1. **`info.dry`** - Game metadata (one per project)
2. **`*.scene.dry`** - Scene files (story nodes)
3. **`*.qdisplay.dry`** - Quick display formatters (value-to-text mapping)

## 🎬 Scene File Structure

### Basic Anatomy

```
# Scene Properties (YAML-like header)
title: Scene Title
subtitle: Optional subtitle
new-page: true|false
tags: tag1, tag2
view-if: condition
on-arrival: code

# Scene Content (Markdown body)
= Heading (optional)

Narrative text with [+ variables +] and [? conditionals ?].

# Subscenes (choices/branches)
@choice_id
title: Choice Title
subtitle: Effects preview
choose-if: condition
unavailable-subtitle: Why unavailable
on-arrival: code
go-to: target_scene

More narrative text...

@another_choice
...
```

## 🔧 Scene Properties Reference

### Display Properties

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `title` | string | Scene title | `title: The Meeting` |
| `subtitle` | string | Scene description | `subtitle: A tense negotiation` |
| `new-page` | boolean | Clear screen? | `new-page: true` |
| `tags` | comma-list | Categorization | `tags: event, political` |

### Card System Properties

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `is-card` | boolean | Is this a deck card? | `is-card: true` |
| `frequency` | 0-1000 | Draw probability | `frequency: 300` (30%) |
| `card-image` | path | Card image | `card-image: img/photo.jpg` |
| `max-visits` | integer | Visit limit | `max-visits: 1` (one-time event) |

### Conditional Properties

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `view-if` | expression | Scene visibility | `view-if: year >= 1930` |
| `choose-if` | expression | Choice availability | `choose-if: resources >= 2` |
| `unavailable-subtitle` | string | Disabled reason | `unavailable-subtitle: Not enough resources` |

### Code Execution

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `on-arrival` | code | Run when entering | `on-arrival: resources += 1` |
| `on-display` | code | Run when displaying | `on-display: calculateStats()` |
| `on-departure` | code | Run when leaving | `on-departure: saveState()` |

### Navigation

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `go-to` | scene-id | Auto-navigate | `go-to: next_scene` |

## 💾 Variable System

### Variable Namespace

All game variables live in the `Q` object (JavaScript):

```javascript
Q.resources = 5           // Access in code
[+ resources +]           // Display in text
```

### Variable Types

```javascript
// Numbers
Q.year = 1928
Q.resources = 2
Q.dissent = 0.05          // Can be float

// Strings
Q.president = "Hindenburg"
Q.chancellor_party = "Z"

// Booleans (use 0/1)
Q.spd_in_government = 1   // true
Q.iron_front_formed = 0   // false

// Arrays
Q.factions = ['left', 'center', 'labor']
Q.election_records = []

// Objects
Q.ministers = {labor: 'SPD', finance: 'Z'}
```

### Variable Naming Conventions (from SPD_DECK)

```javascript
// Singular for single values
Q.dissent, Q.budget, Q.year

// Plurals for counts or percentages
Q.workers = 48           // 48% of population
Q.resources = 3          // 3 resource points

// Party suffixes for party-specific
Q.workers_spd = 60       // 60% of workers support SPD
Q.workers_kpd = 20

// Underscores for compound names
Q.spd_in_government
Q.iron_front_formed
Q.left_dissent

// Timer suffix for cooldowns
Q.fundraising_timer = 6  // 6 months cooldown
```

## 🎯 Conditional Logic

### Operators

```javascript
// Comparison
year = 1930              // Equals
year != 1929             // Not equals
resources >= 2           // Greater or equal
dissent < 0.5            // Less than

// Logical
condition1 and condition2
condition1 or condition2
not condition

// Mathematical (in code blocks)
resources + 2
workers_spd * 1.5
(base_value + modifier) / 2
```

### In Scene Properties

```
view-if: year >= 1930 and unemployment > 10
choose-if: resources >= 2 and not spd_in_government
```

### In Text (Conditional Display)

```
[? if year >= 1930 : The Depression has begun. ?]
[? if not historical_mode : Current polls show... ?]
[? if resources >= 5 : You have abundant resources[? if resources < 3 : Resources are scarce ?]. ?]
```

### In Code Blocks

```javascript
on-arrival: {!
  if (Q.year >= 1930) {
    Q.depression_active = 1;
    Q.unemployment += 5;
  }

  // Ternary
  Q.bonus = Q.dissent < 0.3 ? 5 : 2;
!}
```

## 📝 Code Blocks

### Inline Code (Simple)

```
on-arrival: resources += 1; dissent -= 0.05
```

### Block Code (Complex)

```javascript
on-arrival: {!
  // Full JavaScript available
  Q.resources += 2;
  Q.left_dissent -= 5;

  // Conditionals
  if (Q.in_government) {
    Q.budget -= 1;
  }

  // Loops
  for (var faction of Q.factions) {
    Q[faction + '_dissent'] += 1;
  }

  // Functions
  function calculateSupport() {
    return Q.workers * Q.workers_spd / 100;
  }
  Q.total_support = calculateSupport();
!}
```

### Common Patterns

```javascript
// Modify with bounds
Q.dissent = Math.max(0, Math.min(1, Q.dissent + 0.1));

// Weighted effects
Q.workers_spd += 5 * (1 - Q.dissent);  // Less effective if high dissent

// Conditional effect
Q.bonus = Q.condition ? 10 : 0;

// Array iteration
for (var party of Q.parties) {
  Q[party + '_support'] = 0;
}

// Dynamic property names
var faction = 'left';
Q[faction + '_strength'] += 5;
```

## 🎨 Text Formatting

### Variable Display

```
[+ resources +]                    # Show value: "5"
[+ workers_spd : workers_spd +]    # Format with qdisplay
[+ year +]-[+ month +]             # Combine: "1930-3"
```

### Conditional Text

```
[? if condition : Show if true ?]
[? if not condition : Show if false ?]

# Nested
[? if year >= 1930 : Depression [? if unemployment > 15 : severe ?] ?]

# Multiple conditions
[? if resources >= 5 and dissent < 0.2 : Strong position ?]
```

### Styling (HTML allowed)

```
**bold text**
*italic text*
- Bullet list
1. Numbered list

<span style="color: red;">Red text</span>
```

## 🔄 Scene Flow

### Navigation Methods

1. **Subscene Choice:**
```
@choice_id
go-to: target_scene
```

2. **Auto-navigation:**
```
title: Intermediate Scene
on-arrival: resources += 1
go-to: next_scene     # Auto-proceed
```

3. **Conditional Navigation:**
```javascript
on-arrival: {!
  if (Q.year >= 1933) {
    Q.goto('endgame');
  } else {
    Q.goto('main_loop');
  }
!}
```

### Scene IDs

Scene ID = filename without `.scene.dry`:
- `source/scenes/main.scene.dry` → ID: `main`
- `source/scenes/events/crisis.scene.dry` → ID: `crisis`

### Subscene IDs

Format: `scene_id.subscene_name`
- Scene: `main`, Subscene: `@continue` → `main.continue`

## 🃏 Card/Deck System

### How Cards Work

1. **Marking as Card:**
```
is-card: true
frequency: 300         # 30% chance to appear
```

2. **Visibility Conditions:**
```
view-if: timer <= 0 and year >= 1930
```

3. **Cooldown Management:**
```
on-arrival: fundraising_timer = 6    # 6 month cooldown
```

4. **Draw Priority:**
- Higher `frequency` = more likely
- `frequency: 0` = never drawn (but visible if view-if true)
- Pinned cards (advisors): always visible, bypass deck

### Card States

```
NOT IN DECK → view-if: false
    ↓
IN DECK → view-if: true, frequency > 0
    ↓
DRAWN → Player sees card
    ↓
SELECTED → Player clicks choice
    ↓
EXECUTED → on-arrival runs
    ↓
COOLDOWN → timer > 0
    ↓
[Back to IN DECK when timer <= 0]
```

## 🎲 Quick Displays (qdisplay)

### Format

```
# filename.qdisplay.dry
(..-5) very low
(-5..0) low
(0..5) medium
(5..10) high
(10..) very high
```

### Usage

```
# In scene property
on-arrival: workers_spd_display = 'medium'

# In text
[+ workers_spd : workers_spd +]    # Uses workers_spd.qdisplay.dry
```

### Range Syntax

```
(..-5)      # x < -5
(-5..0)     # -5 <= x < 0
(0..5)      # 0 <= x < 5
(5..)       # x >= 5
```

## 🔊 Media

### Audio

```
audio: shuffle music/track1.mp3 music/track2.mp3 music/track3.mp3
```

### Images

```
card-image: img/photo.jpg

# In text (HTML)
<img src="img/photo.jpg" alt="Description">
```

## ⚡ Performance Tips

1. **Minimize code in on-display** (runs every render)
2. **Use on-arrival for one-time calculations**
3. **Cache computed values in variables**
4. **Limit nested loops**

## 🐛 Common Pitfalls

### 1. Variable Scope

```javascript
// WRONG - local variable
on-arrival: {!
  var x = 5;    // Lost after execution
!}

// RIGHT - game variable
on-arrival: {!
  Q.x = 5;      // Persists
!}
```

### 2. Comparison vs Assignment

```javascript
// WRONG - assignment (always true)
view-if: year = 1930

// RIGHT - comparison
view-if: year == 1930  // or just: year = 1930 (Dendry allows)
```

### 3. String Quotes in Properties

```
# WRONG
choose-if: president = Hindenburg

# RIGHT
choose-if: president = "Hindenburg"
```

### 4. Division by Zero

```javascript
// WRONG
Q.ratio = Q.a / Q.b;

// RIGHT
Q.ratio = Q.b > 0 ? Q.a / Q.b : 0;
```

### 5. Array/Object Modification

```javascript
// Be careful with references
Q.original = [1, 2, 3];
Q.copy = Q.original;        // Same reference!
Q.copy.push(4);             // Modifies original too

// Use spread for copy
Q.copy = [...Q.original];   // Independent copy
```

## 📚 Advanced Features

### Achievements

```javascript
on-arrival: achievement: wirtschaftspolitik
```

### Save/Load

- Automatic in DendryNexus
- Can disable with: `historical_mode: true` in info.dry

### Mods

- `mod_loader.scene.dry` for custom content

---

**Next Steps:**
- Review [02-development-guide.md](02-development-guide.md) for workflow
- See [03-patterns.md](03-patterns.md) for templates and examples
