# DendryNexus Development Guide

**Purpose:** Practical workflow for creating and testing DendryNexus games.

## 🏗️ Project Setup

### Directory Structure

```
my-game/
├── source/
│   ├── info.dry                 # REQUIRED: Game metadata
│   ├── scenes/
│   │   ├── root.scene.dry       # REQUIRED: Entry point
│   │   ├── main.scene.dry       # Main game loop
│   │   └── ...
│   └── qdisplays/
│       └── *.qdisplay.dry
├── out/
│   └── html/                    # Compiled output
├── package.json
└── node_modules/
```

### Minimal Setup

**1. Create `source/info.dry`:**
```
title: My Game Title
author: Your Name
ifid: GENERATE-UNIQUE-ID      # Use UUID generator
```

**2. Create `source/scenes/root.scene.dry`:**
```
title: Start
new-page: true

on-arrival: {!
  // Initialize game variables
  Q.resources = 5;
  Q.time = 0;
!}

= Welcome

Welcome to the game!

@start
title: Begin
go-to: main
```

**3. Create `source/scenes/main.scene.dry`:**
```
title: Main Scene
new-page: true

= Main Game

Current resources: [+ resources +]

@action1
title: Do something
on-arrival: resources += 1

You did something!

@back
title: Continue
go-to: main
```

## 🛠️ Development Workflow

### Step 1: Design on Paper

**Before coding, answer:**
1. What is the core loop? (monthly turns, event-driven, etc.)
2. What are the key variables? (resources, relationships, stats)
3. What are the major branches? (win/loss conditions)
4. What is the card/event structure?

**Example (Political Game):**
```
Core Loop: Monthly decision-making
Variables:
  - resources (0-10)
  - public_support (0-100)
  - dissent (0-1)
  - year, month
Branches:
  - Win: public_support > 70 by year 5
  - Lose: dissent > 0.8 OR public_support < 20
Cards:
  - Party affairs (always available)
  - Government affairs (if in power)
  - Events (triggered by conditions)
```

### Step 2: Initialize Variables (`root.scene.dry`)

```javascript
on-arrival: {!
  // === Time ===
  Q.time = 0;
  Q.year = 2020;
  Q.month = 1;

  // === Resources ===
  Q.resources = 5;          // Starting resources
  Q.income = 2;             // Monthly income

  // === Core Stats ===
  Q.public_support = 50;    // 0-100
  Q.dissent = 0.1;          // 0-1

  // === Factions (if applicable) ===
  Q.factions = ['moderates', 'radicals'];
  Q.moderates_strength = 60;
  Q.radicals_strength = 40;
  Q.moderates_dissent = 20;
  Q.radicals_dissent = 30;

  // === Flags ===
  Q.in_government = 0;
  Q.policy_a_adopted = 0;

  // === Timers (for card cooldowns) ===
  Q.fundraising_timer = 0;
  Q.rally_timer = 0;

  // === Arrays for history tracking ===
  Q.event_history = [];
  Q.stat_records = [];
!}
```

### Step 3: Create Main Loop

```
# main.scene.dry
title: Month [+ month +], [+ year +]
new-page: true
on-arrival: month_actions = 0
go-to: post_month if month_actions > 0

= [+ year +] - Month [+ month +]

**Resources:** [+ resources +]
**Public Support:** [+ public_support +]%
**Dissent:** [+ dissent : dissent +]

[? if month_actions = 0 : You may take one action this month. ?]
[? if month_actions > 0 : Processing month... ?]
```

### Step 4: Create Cards

**Template: Party Affairs Card**
```
# source/scenes/party_affairs/fundraising.scene.dry

title: Fundraising
subtitle: Raise resources
frequency: 400                          # 40% draw chance
view-if: fundraising_timer <= 0         # Only when off cooldown
on-arrival: fundraising_timer = 6; month_actions += 1
is-card: true
tags: party_affairs

= Fundraising

Your party needs funds to operate effectively.

@maintain
title: Maintain current contribution levels
on-arrival: {}

You keep things as they are.

@increase
title: Increase contributions
subtitle: +2 resources, -5 public support
choose-if: public_support > 20
on-arrival: resources += 2; public_support -= 5

Members grumble but pay up.

@reduce
title: Reduce contributions
subtitle: +5 public support, -1 resources
on-arrival: resources -= 1; public_support += 5

Members are happier with lower dues.
```

**Template: Event Card**
```
# source/scenes/events/economic_crisis.scene.dry

title: Economic Crisis
view-if: year = 2022 and month = 6      # Specific date
max-visits: 1                           # One-time event
on-arrival: month_actions += 1
is-card: true
tags: event

= Economic Crisis Strikes

The economy has taken a downturn!

@austerity
title: Implement austerity measures
subtitle: -3 public support, +1 resources
on-arrival: public_support -= 3; resources += 1; policy_austerity = 1

You cut spending to balance the budget.

@stimulus
title: Stimulus spending
subtitle: -2 resources, +2 public support
choose-if: resources >= 2
on-arrival: resources -= 2; public_support += 2; policy_stimulus = 1

You spend to help the economy.
```

### Step 5: Create Post-Processing Scene

```
# post_month.scene.dry
title: End of Month
on-arrival: {!
  // === Time advancement ===
  Q.month += 1;
  if (Q.month > 12) {
    Q.month = 1;
    Q.year += 1;
  }
  Q.time += 1;

  // === Income ===
  Q.resources += Q.income;

  // === Decay timers ===
  var timers = ['fundraising_timer', 'rally_timer'];
  for (var timer of timers) {
    if (Q[timer] > 0) {
      Q[timer] -= 1;
    }
  }

  // === Calculate derived stats ===
  // Example: dissent affects support decay
  Q.public_support += -Q.dissent * 5;

  // === Bounds checking ===
  Q.resources = Math.max(0, Q.resources);
  Q.public_support = Math.max(0, Math.min(100, Q.public_support));
  Q.dissent = Math.max(0, Math.min(1, Q.dissent));

  // === Record history ===
  Q.stat_records.push({
    time: Q.time,
    support: Q.public_support,
    dissent: Q.dissent
  });

  // === Check win/loss conditions ===
  if (Q.dissent > 0.8) {
    Q.goto('game_over_dissent');
  } else if (Q.public_support < 10) {
    Q.goto('game_over_unpopular');
  } else if (Q.year > 2025) {
    Q.goto('game_over_time_limit');
  }
!}
go-to: main
```

### Step 6: Create Game Over Scenes

```
# game_over_dissent.scene.dry
title: Game Over
new-page: true

= Collapse from Within

Your organization has torn itself apart from internal dissent.

Final Stats:
- Public Support: [+ public_support +]%
- Internal Dissent: [+ dissent : dissent +]
- Resources: [+ resources +]

@restart
title: Try Again
go-to: root
```

## 🧪 Testing Workflow

### Local Testing

```bash
# Build the game
npm run build

# Open in browser
open out/html/index.html    # macOS
xdg-open out/html/index.html # Linux
start out/html/index.html    # Windows
```

### Testing Checklist

**Variables:**
- [ ] All variables initialized in `root.scene.dry`
- [ ] Variables bounded (no negative resources, etc.)
- [ ] Variables reset between games

**Cards:**
- [ ] Cards appear when `view-if` is true
- [ ] Cards disappear on cooldown (timer > 0)
- [ ] Card effects modify correct variables
- [ ] `max-visits` enforced for one-time events

**Flow:**
- [ ] Game starts from `root.scene.dry`
- [ ] Main loop returns to main scene
- [ ] Game over conditions work
- [ ] All `go-to` targets exist

**Balance:**
- [ ] Game not too easy/hard
- [ ] Resources sufficient but scarce
- [ ] Choices have meaningful tradeoffs

### Debugging Tips

**1. Check browser console (F12) for:**
- JavaScript errors in `{! code !}` blocks
- Missing scene references
- Variable typos

**2. Add debug display:**
```
# Add to main.scene.dry
[? if debug_mode = 1 :
  **DEBUG:**
  - Time: [+ time +]
  - Resources: [+ resources +]
  - Fundraising timer: [+ fundraising_timer +]
?]
```

**3. Enable debug mode:**
```javascript
# In root.scene.dry
Q.debug_mode = 1;    // Set to 0 for production
```

**4. Test edge cases:**
```javascript
# Temporarily in root.scene.dry for testing
Q.resources = 0;       // Test with no resources
Q.dissent = 0.9;      // Test near game over
Q.year = 2024;        // Skip ahead
```

## 🎯 Common Patterns

### Pattern: Monthly Income

```javascript
# In post_month.scene.dry
on-arrival: {!
  Q.resources += Q.income;
!}
```

### Pattern: Timer Cooldown

```javascript
# In card
view-if: action_timer <= 0
on-arrival: action_timer = 6    # 6 month cooldown

# In post_month.scene.dry
if (Q.action_timer > 0) {
  Q.action_timer -= 1;
}
```

### Pattern: Weighted Effects

```javascript
# Higher dissent reduces effectiveness
on-arrival: public_support += 10 * (1 - dissent)
```

### Pattern: Faction Management

```javascript
# Calculate aggregate dissent
var total_strength = 0;
var weighted_dissent = 0;
for (var faction of Q.factions) {
  total_strength += Q[faction + '_strength'];
  weighted_dissent += Q[faction + '_strength'] * Q[faction + '_dissent'];
}
Q.dissent = weighted_dissent / (total_strength * 100);
```

### Pattern: Dynamic Difficulty

```javascript
# In post_month.scene.dry
// Increase pressure over time
Q.dissent_growth = 0.01 * (Q.year - 2020);
Q.dissent += Q.dissent_growth;
```

### Pattern: Achievement Tracking

```javascript
# When condition met
if (Q.public_support >= 80 && !Q.achievement_popular) {
  Q.achievement_popular = 1;
  Q.achievement('popular_leader');
}
```

## 📝 Iteration Tips

### Version Control

```bash
git init
git add source/
git commit -m "Initial game structure"

# After major milestones
git commit -m "Added economic crisis event chain"
```

### Playtesting Cycle

1. **Write 5-10 scenes**
2. **Build and play through**
3. **Note issues:**
   - Confusing text
   - Broken flows
   - Balance problems
4. **Fix and repeat**

### Content Organization

```
source/scenes/
├── root.scene.dry
├── main.scene.dry
├── post_month.scene.dry
├── library.scene.dry        # Help/documentation
├── status.scene.dry         # Detailed stats view
├── party_affairs/           # Regular actions
│   ├── fundraising.scene.dry
│   ├── rally.scene.dry
│   └── ...
├── government_affairs/      # Special actions (conditional)
│   ├── legislation.scene.dry
│   └── ...
├── events/                  # Triggered events
│   ├── 2020/
│   │   ├── crisis_a.scene.dry
│   │   └── crisis_b.scene.dry
│   └── 2021/
│       └── ...
└── game_over/
    ├── victory.scene.dry
    └── defeat.scene.dry
```

## 🚀 Publishing

### Build for Distribution

```bash
npm run build

# Output is in: out/html/
# - index.html (main file)
# - game.js (engine)
# - content.js (your game data)
```

### Host on GitHub Pages

1. Push to GitHub
2. Settings → Pages → Source: `out/html/`
3. Game live at: `https://username.github.io/repo-name/`

### Export as Zip

```bash
cd out/html
zip -r ../../my-game.zip *
```

## 📊 Analytics (Optional)

Track player choices for balance:

```javascript
# In scenes
on-arrival: {!
  if (window.gtag) {
    gtag('event', 'choice_made', {
      scene: 'fundraising',
      choice: 'increase'
    });
  }
!}
```

---

**Next Steps:**
- Review [03-patterns.md](03-patterns.md) for reusable templates
- See [04-brainstorming.md](04-brainstorming.md) for design process
