# DendryNexus Game Development Documentation

Welcome to the comprehensive documentation system for AI-assisted DendryNexus game development!

## 📚 What Is This?

This documentation system helps you (and AI assistants) create interactive narrative games using **DendryNexus**, a powerful framework for choice-based games with complex mechanics.

## 🎯 Who Is This For?

- **Game developers** creating DendryNexus games
- **AI assistants** (like Claude) helping with game development
- **Writers** designing interactive narratives
- **Designers** planning game mechanics and systems

## 📁 Documentation Structure

```
docs/
├── README.md (you are here)
│
├── ai-guides/              # Technical reference for AI assistants
│   ├── 00-index.md        # Navigation guide
│   ├── 01-architecture.md # DendryNexus framework deep dive
│   ├── 02-development-guide.md # Practical workflow
│   ├── 03-patterns.md     # Reusable templates and code
│   ├── 04-brainstorming.md # Design ideation framework
│   └── 05-ai-context.md   # How AI should use these docs
│
├── templates/              # Ready-to-use .dry file templates
│   ├── scene-basic.dry    # Simple narrative scene
│   ├── scene-card.dry     # Deck card with cooldown
│   ├── scene-event.dry    # One-time triggered event
│   └── scene-advisor.dry  # Character with special ability
│
└── game-design/           # Your game's design documents
    ├── concept.md         # High-level vision (FILL THIS OUT)
    ├── mechanics.md       # Systems and rules (FILL THIS OUT)
    ├── narrative.md       # Story structure (FILL THIS OUT)
    └── characters.md      # Character details (FILL THIS OUT)
```

## 🚀 Quick Start

### For Developers

1. **Read the concept:**
   - Start with `ai-guides/00-index.md` for overview
   - Read `ai-guides/01-architecture.md` to understand DendryNexus

2. **Plan your game:**
   - Use `ai-guides/04-brainstorming.md` as a guide
   - Fill out templates in `game-design/` folder
   - Use slash command `/brainstorm` with AI assistant

3. **Build your game:**
   - Use templates from `templates/` folder
   - Reference patterns in `ai-guides/03-patterns.md`
   - Follow workflow in `ai-guides/02-development-guide.md`
   - Use slash command `/new-scene` to create scenes

4. **Validate and iterate:**
   - Use slash command `/validate` to check your work
   - Playtest frequently
   - Refine based on feedback

---

### For AI Assistants

1. **Understand your role:**
   - Read `ai-guides/05-ai-context.md` first
   - This explains how to use all other documentation

2. **Reference architecture:**
   - Use `ai-guides/01-architecture.md` for technical questions
   - Use `ai-guides/03-patterns.md` for code examples

3. **Follow workflows:**
   - Brainstorming: `ai-guides/04-brainstorming.md`
   - Development: `ai-guides/02-development-guide.md`
   - Validation: Checklist in `ai-guides/05-ai-context.md`

4. **Use slash commands:**
   - `/brainstorm` - Guide user through game design
   - `/new-scene` - Create new scene from template
   - `/validate` - Check code for errors and best practices

---

## 🎮 Example Game: SPD_DECK

This repository includes **SPD_DECK**, a complete DendryNexus game about Weimar Germany (1928-1933). Use it as a reference for:

- Complex variable management (200+ variables)
- Deck-based card system
- Historical event triggers
- Faction management
- Economic simulation
- Multiple endings

**Key files to study:**
- `source/scenes/root.scene.dry` - Variable initialization
- `source/scenes/main.scene.dry` - Main game loop
- `source/scenes/post_event.scene.dry` - State recalculation
- `source/scenes/party_affairs/` - Example regular cards
- `source/scenes/events/` - Example one-time events

---

## 🛠️ Slash Commands

Custom Claude commands for rapid development:

### `/brainstorm`
Start a structured brainstorming session for your game.
- Guides through concept, mechanics, and narrative design
- Creates design documents
- Asks clarifying questions

**When to use:** Starting a new game or expanding existing one

---

### `/new-scene`
Create a new scene using templates.
- Asks about scene type and purpose
- Generates complete `.dry` file
- Provides implementation instructions

**When to use:** Adding content to your game

---

### `/validate`
Validate game content for errors and best practices.
- Checks syntax, logic, and balance
- Provides detailed error reports
- Suggests improvements

**When to use:** Reviewing scenes or debugging issues

---

## 📖 How to Use This Documentation

### Scenario 1: I'm brand new to DendryNexus

1. Read `ai-guides/01-architecture.md` (30 min)
2. Skim `ai-guides/03-patterns.md` for examples (15 min)
3. Try creating a simple scene using `templates/scene-basic.dry`
4. Build and test it following `ai-guides/02-development-guide.md`

**Time investment:** ~1-2 hours
**Result:** Basic understanding + first working scene

---

### Scenario 2: I want to design a new game

1. Read `ai-guides/04-brainstorming.md` (20 min)
2. Work through the brainstorming framework (1-3 hours)
3. Fill out `game-design/concept.md` and `game-design/mechanics.md`
4. Start prototyping core loop using templates

**Time investment:** 3-5 hours
**Result:** Complete game design document + prototype plan

---

### Scenario 3: I'm stuck on a specific mechanic

1. Check `ai-guides/03-patterns.md` for similar patterns
2. Review `ai-guides/01-architecture.md` for technical details
3. Look at SPD_DECK source code for complex examples
4. Ask AI assistant with `/validate` command

**Time investment:** 15-60 min
**Result:** Working implementation or clear path forward

---

### Scenario 4: I want AI to help me develop

1. Ensure AI has read `ai-guides/05-ai-context.md`
2. Share your `game-design/` documents with AI
3. Use slash commands (`/brainstorm`, `/new-scene`, `/validate`)
4. Iterate on AI suggestions

**Time investment:** Ongoing collaboration
**Result:** Faster development with AI assistance

---

## 💡 Best Practices

### Do's ✅

- **Document your design first** - Fill out `game-design/` templates before coding
- **Start small** - Build core loop with 10-20 scenes, then expand
- **Use templates** - Don't reinvent the wheel, adapt existing patterns
- **Test frequently** - Build and play after every major addition
- **Version control** - Use git to track changes
- **Iterate** - First draft won't be perfect, refine based on playtesting

### Don'ts ❌

- **Don't skip initialization** - Always define variables in `root.scene.dry`
- **Don't forget bounds checking** - Resources can go negative without Math.max/min
- **Don't create walls of text** - Break narrative into readable chunks
- **Don't make choices without consequences** - Every choice should matter
- **Don't optimize prematurely** - Get it working, then make it better
- **Don't develop in isolation** - Get feedback early and often

---

## 🎓 Learning Path

### Beginner (0-10 hours experience)
1. ✅ Understand `.dry` file format
2. ✅ Create basic scenes with choices
3. ✅ Implement simple variables and conditions
4. ✅ Build a 5-10 scene prototype

**Resources:** `01-architecture.md`, `templates/scene-basic.dry`

---

### Intermediate (10-50 hours experience)
1. ✅ Implement card/deck system
2. ✅ Create complex conditional logic
3. ✅ Build multi-variable systems (factions, economy)
4. ✅ Design branching narratives

**Resources:** `03-patterns.md`, SPD_DECK examples

---

### Advanced (50+ hours experience)
1. ✅ Optimize for performance and user experience
2. ✅ Create emergent gameplay through system interactions
3. ✅ Build tools or extensions for DendryNexus
4. ✅ Ship complete games

**Resources:** SPD_DECK source code, DendryNexus GitHub

---

## 🔗 External Resources

- **DendryNexus GitHub:** https://github.com/aucchen/dendrynexus
- **Original Dendry:** https://github.com/idmillington/dendry
- **DendryNexus Documentation:** (Check GitHub README)

---

## 🤝 Contributing to Documentation

Found an error? Have a suggestion? Want to add a pattern?

1. Note the issue or improvement
2. Update the relevant documentation file
3. Test that examples still work
4. Share with the community (if applicable)

---

## 📝 Your Next Steps

### If you're just exploring:
→ Read `ai-guides/01-architecture.md` to understand DendryNexus

### If you want to design a game:
→ Use `ai-guides/04-brainstorming.md` and fill out `game-design/concept.md`

### If you're ready to build:
→ Use templates in `templates/` and reference `ai-guides/03-patterns.md`

### If you need help:
→ Ask AI assistant using slash commands (`/brainstorm`, `/new-scene`, `/validate`)

---

## 🎉 Success Stories

Track your progress here:

- [ ] Created first working scene
- [ ] Built complete core loop
- [ ] Implemented card/deck system
- [ ] Completed game design document
- [ ] First playable prototype
- [ ] Content complete (all scenes written)
- [ ] Playtest with others
- [ ] Published/shared game

---

**Remember:** Every great game starts with a single scene. Build incrementally, test frequently, and iterate based on feedback. You've got this! 🚀

**Happy game developing!**
