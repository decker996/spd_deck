# DendryNexus Patterns and Templates

**Purpose:** Reusable templates and common patterns for rapid development.

## 📋 Scene Templates

### Template 1: Basic Scene

```
title: Scene Title
subtitle: Brief description
new-page: true
tags: category

= Heading

Narrative text describing the situation.

@choice1
title: First Choice
subtitle: Effects summary
choose-if: condition
unavailable-subtitle: Reason if unavailable
on-arrival: variable += 1

Result narrative...

@choice2
title: Second Choice
on-arrival: other_variable -= 1

Alternative result...
```

**Use Case:** Simple narrative branches, dialog scenes

---

### Template 2: Card with Cooldown

```
title: Card Name
frequency: 300                      # 30% draw probability
view-if: card_timer <= 0           # Only when cooled down
on-arrival: card_timer = 6; month_actions += 1
is-card: true
tags: party_affairs

= Card Title

Description of the situation.

@option_a
title: Conservative Option
subtitle: Safe choice with modest gains
on-arrival: resources += 1

Safe result...

@option_b
title: Risky Option
subtitle: High risk, high reward
choose-if: resources >= 2
unavailable-subtitle: Not enough resources (need 2)
on-arrival: resources -= 2; public_support += 10 if success else public_support -= 5

Risk result...
```

**Use Case:** Regular repeatable actions (fundraising, rallies, media)

---

### Template 3: One-Time Event

```
title: Historical Event
view-if: year = 2025 and month = 6     # Specific trigger
max-visits: 1                          # Only once
on-arrival: month_actions += 1; event_flag = 1
is-card: true
tags: event, crisis

= Event Title

[? if previous_choice = "A" : Special text for path A ?]
[? if previous_choice = "B" : Special text for path B ?]

Major event description...

@response_calm
title: Measured Response
subtitle: Moderate effects
on-arrival: public_support += 5; dissent += 0.05

Calm response result...

@response_bold
title: Bold Action
subtitle: High stakes
on-arrival: {!
  if (Q.public_support > 50) {
    Q.public_support += 15;
    Q.resources += 2;
  } else {
    Q.public_support -= 10;
    Q.dissent += 0.15;
  }
!}

[? if public_support > 50 : Bold move pays off! ?]
[? if not public_support > 50 : Bold move backfires... ?]
```

**Use Case:** Major story events, crises, plot milestones

---

### Template 4: Advisor/Character Card

```
title: Advisor Name
subtitle: Their expertise/role
frequency: 0                           # Pinned, always visible
view-if: advisor_recruited = 1         # Only if recruited
on-arrival: advisor_action_timer = 6; month_actions += 1
is-card: true
tags: advisor
card-image: img/advisors/name.jpg

= Advisor Name

Brief bio and current advice.

@action_1
title: Their Signature Action
subtitle: Uses their expertise
choose-if: advisor_action_timer <= 0
unavailable-subtitle: [+ advisor_name +] needs time to prepare (cooldown: [+ advisor_action_timer +] months)
on-arrival: {!
  // Complex action with expertise bonus
  var base_effect = 5;
  var expertise_bonus = 3;
  Q.public_support += base_effect + expertise_bonus;
!}

Result of expert action...

@consult
title: Consult for advice
on-arrival: {}

[? if current_crisis = 1 : Specific advice for crisis ?]
[? if not current_crisis : General strategic advice ?]
```

**Use Case:** Persistent characters with special abilities

---

### Template 5: Status/Info Screen

```
title: Status Overview
subtitle: Current game state
new-page: true
tags: meta

= Status Report

## Resources
- **Resources:** [+ resources +]
- **Monthly Income:** [+ income +]

## Political Situation
- **Public Support:** [+ public_support +]%
- **Internal Dissent:** [+ dissent : dissent +]

[? if in_government = 1 :
  ## Government
  - **Position:** In power
  - **Coalition Partners:** [+ partners +]
?]

## Key Flags
[? if policy_a = 1 : - Policy A implemented ?]
[? if policy_b = 1 : - Policy B implemented ?]

@back
title: Return
go-to: main
```

**Use Case:** Status screens, help pages, reference info

---

## 🎯 Common Patterns

### Pattern: Resource Management

```javascript
// Basic exchange
on-arrival: resources -= 2; public_support += 10

// Conditional cost
choose-if: resources >= 3
on-arrival: resources -= 3; major_policy = 1

// Variable cost based on state
on-arrival: {!
  var cost = Q.in_government ? 2 : 4;  // Cheaper in government
  Q.resources -= cost;
!}

// Efficiency based on support
on-arrival: {!
  var efficiency = Q.public_support / 100;
  var gained = Math.floor(5 * efficiency);
  Q.resources += gained;
!}
```

---

### Pattern: Faction Management

```javascript
// Simple faction satisfaction
on-arrival: moderates_dissent -= 5; radicals_dissent += 3

// Proportional to strength
on-arrival: {!
  var effect = 5 * (Q.moderates_strength / 100);
  Q.moderates_dissent -= effect;
!}

// Calculate aggregate dissent
on-arrival: {!
  var total = 0;
  var weighted = 0;
  for (var faction of Q.factions) {
    var strength = Q[faction + '_strength'];
    var dissent = Q[faction + '_dissent'];
    total += strength;
    weighted += strength * dissent;
  }
  Q.overall_dissent = weighted / (total * 100);
!}

// Balance factions (zero-sum)
on-arrival: {!
  Q.moderates_strength += 5;
  Q.radicals_strength -= 5;
  // Ensure bounds
  Q.moderates_strength = Math.max(0, Math.min(100, Q.moderates_strength));
  Q.radicals_strength = Math.max(0, Math.min(100, Q.radicals_strength));
!}
```

---

### Pattern: Relationship System

```javascript
// Simple relationship change
on-arrival: ally_relation += 10; enemy_relation -= 5

// Relationship affects effectiveness
on-arrival: {!
  var base = 10;
  var relation_modifier = Q.ally_relation / 100;  // 0-1 scale
  var total = base * (1 + relation_modifier);
  Q.public_support += total;
!}

// Relationship gates (thresholds)
choose-if: ally_relation >= 60
unavailable-subtitle: Relationship too low (need 60, have [+ ally_relation +])

// Dynamic text based on relationship
= Meeting

[? if ally_relation >= 75 : They greet you warmly. ?]
[? if ally_relation >= 50 and ally_relation < 75 : They are cordial but reserved. ?]
[? if ally_relation >= 25 and ally_relation < 50 : They are cold and distant. ?]
[? if ally_relation < 25 : They refuse to meet with you. ?]
```

---

### Pattern: Time-Based Events

```javascript
// Specific date trigger
view-if: year = 2025 and month = 3

// Date range
view-if: year >= 2024 and year <= 2026

// Seasonal
view-if: month >= 6 and month <= 8  // Summer

// Anniversary/recurring
view-if: month = 1 and year >= 2022  // Every January after 2022

// Countdown to deadline
= Status

[? if year < 2030 : Years until deadline: [+ 2030 - year +] ?]
[? if year = 2030 : This is the final year! ?]
```

---

### Pattern: Cascading Effects

```javascript
on-arrival: {!
  // Initial action
  Q.resources -= 3;
  Q.policy_implemented = 1;

  // Secondary effects
  Q.public_support += 10;
  Q.dissent -= 0.05;

  // Tertiary effects (conditional)
  if (Q.in_government) {
    Q.government_stability += 5;
  }

  // Faction reactions
  Q.moderates_dissent -= 5;  // Happy
  Q.radicals_dissent += 8;   // Unhappy

  // International reactions
  Q.ally_relation += 5;
  Q.rival_relation -= 3;

  // Economic impact
  Q.economic_growth += 0.5;
  Q.unemployment -= 1;

  // Long-term flags
  Q.historical_significance += 1;
!}
```

---

### Pattern: Probability and Randomness

```javascript
// Simple coin flip
on-arrival: {!
  if (Math.random() < 0.5) {
    Q.success = 1;
    Q.public_support += 10;
  } else {
    Q.success = 0;
    Q.public_support -= 5;
  }
!}

// Weighted probability (based on stat)
on-arrival: {!
  var success_chance = Q.public_support / 100;  // 0-1
  if (Math.random() < success_chance) {
    Q.resources += 5;
  }
!}

// Multiple outcomes
on-arrival: {!
  var roll = Math.random();
  if (roll < 0.2) {
    Q.outcome = 'great_success';
    Q.resources += 10;
  } else if (roll < 0.7) {
    Q.outcome = 'success';
    Q.resources += 5;
  } else {
    Q.outcome = 'failure';
    Q.resources -= 2;
  }
!}

// Display result
[? if outcome = 'great_success' : Incredible success! ?]
[? if outcome = 'success' : Things went well. ?]
[? if outcome = 'failure' : It didn't work out. ?]
```

---

### Pattern: Stat Normalization

```javascript
// In post_month.scene.dry or similar
on-arrival: {!
  // Support across groups must sum to 100%
  var groups = ['workers', 'middle_class', 'elites'];
  var parties = ['party_a', 'party_b', 'party_c'];

  for (var group of groups) {
    var total = 0;
    for (var party of parties) {
      total += Q[group + '_' + party];
    }

    // Normalize
    for (var party of parties) {
      var key = group + '_' + party;
      Q[key + '_normalized'] = 100 * Q[key] / total;
    }
  }

  // Calculate overall support
  for (var party of parties) {
    var support = 0;
    for (var group of groups) {
      var group_size = Q[group + '_size'];  // e.g., 40%
      var group_support = Q[group + '_' + party + '_normalized'];
      support += group_size * group_support / 100;
    }
    Q[party + '_total_support'] = support;
  }
!}
```

---

### Pattern: Complex Conditionals

```javascript
// Multiple AND conditions
choose-if: resources >= 3 and public_support >= 50 and not in_crisis

// OR conditions (use code block)
choose-if: resources >= 5 or has_emergency_fund

// Complex combinations
on-arrival: {!
  // Custom logic
  var can_proceed = false;

  if (Q.resources >= 3 && Q.public_support >= 50) {
    can_proceed = true;
  }

  if (Q.emergency_powers && Q.crisis_active) {
    can_proceed = true;
  }

  if (!can_proceed) {
    Q.goto('cannot_proceed');
  } else {
    Q.goto('proceed');
  }
!}
```

---

## 🧩 Subsystem Templates

### Economy Subsystem

```javascript
// In root.scene.dry
Q.inflation = 2.0;          // %
Q.unemployment = 5.0;       // %
Q.gdp_growth = 3.0;         // %
Q.budget = 5;               // Government budget (0-10)

// In post_month.scene.dry
on-arrival: {!
  // Economic growth affects unemployment
  Q.unemployment -= Q.gdp_growth * 0.3;
  Q.unemployment = Math.max(0, Q.unemployment);

  // Unemployment affects support
  Q.public_support -= Q.unemployment * 0.5;

  // Budget affects spending capacity
  Q.monthly_budget = Math.floor(Q.budget / 12);

  // Inflation affects real income
  var real_growth = Q.gdp_growth - Q.inflation;
  if (real_growth < 0) {
    Q.public_support -= 2;
  }
!}
```

---

### Combat/Conflict Subsystem

```javascript
// Strength-based resolution
on-arrival: {!
  var our_strength = Q.military_strength;
  var their_strength = Q.enemy_strength;

  var our_chance = our_strength / (our_strength + their_strength);

  if (Math.random() < our_chance) {
    Q.victory = 1;
    Q.territory += 1;
    Q.resources += 3;
  } else {
    Q.victory = 0;
    Q.territory -= 1;
    Q.military_strength -= 2;
  }

  // Both sides take casualties
  Q.military_strength -= 1;
  Q.enemy_strength -= 1;
!}
```

---

### Narrative Choice Memory

```javascript
// Track important choices
on-arrival: {!
  if (!Q.choice_history) {
    Q.choice_history = [];
  }

  Q.choice_history.push({
    time: Q.time,
    scene: 'crisis_meeting',
    choice: 'compromise',
    year: Q.year
  });
!}

// Reference past choices
[? if choice_history_includes_compromise :
  You remember your previous compromise...
?]

// Helper function (in root.scene.dry)
on-arrival: {!
  Q.choice_history_includes = function(choice_id) {
    if (!Q.choice_history) return false;
    return Q.choice_history.some(c => c.choice === choice_id);
  };
!}
```

---

## 📚 Full Example: Card Collection

### Party Affairs: Fundraising
```
title: Fundraising
frequency: 400
view-if: fundraising_timer <= 0
on-arrival: fundraising_timer = 6; month_actions += 1
is-card: true
tags: party_affairs

= Fundraising Campaign

The party treasury needs replenishment.

@standard
title: Standard campaign
subtitle: +2 resources, moderate effort
on-arrival: resources += 2

@aggressive
title: Aggressive campaign
subtitle: +4 resources, -8 public support
on-arrival: resources += 4; public_support -= 8

@reduce_burden
title: Reduce member contributions
subtitle: -1 resources, +5 public support
on-arrival: resources -= 1; public_support += 5
```

### Government Affairs: Legislation
```
title: Major Legislation
frequency: 200
view-if: in_government and legislation_timer <= 0
on-arrival: legislation_timer = 12; month_actions += 1
is-card: true
tags: government_affairs

= Legislative Agenda

You have the opportunity to push major legislation.

@reform_a
title: Social reform
subtitle: -3 budget, +15 public support (workers)
choose-if: budget >= 3
on-arrival: budget -= 3; workers_support += 15; reform_a_passed = 1

@reform_b
title: Economic reform
subtitle: -4 budget, +2 gdp growth
choose-if: budget >= 4
on-arrival: budget -= 4; gdp_growth += 2; reform_b_passed = 1
```

### Event: Crisis
```
title: Political Crisis
view-if: year = 2024 and month = 9
max-visits: 1
on-arrival: month_actions += 1; crisis_2024 = 1
is-card: true
tags: event, crisis

= Crisis Erupts

A major scandal has rocked the government!

@damage_control
title: Damage control
subtitle: -2 resources, limit support loss
on-arrival: resources -= 2; public_support -= 10

@full_investigation
title: Launch investigation
subtitle: -4 resources, potential long-term gain
choose-if: resources >= 4
on-arrival: resources -= 4; investigation_ongoing = 1
go-to: crisis_investigation_results
```

---

**Next:** See [04-brainstorming.md](04-brainstorming.md) for design ideation process.
