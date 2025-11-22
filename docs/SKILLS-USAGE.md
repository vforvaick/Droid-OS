# Skills System - Usage Guide

Complete guide for using the proven methodologies system integrated into Droid-OS.

---

## 📚 What Are Skills?

Skills are **proven methodologies** that droids can invoke to systematically approach common challenges:
- **Testing**: TDD, async testing patterns, anti-pattern avoidance
- **Debugging**: Root cause analysis, systematic investigation, verification
- **Collaboration**: Brainstorming, planning, code review workflows
- **Meta**: Creating and sharing new skills

---

## 🎯 How Skills Work

### Self-Organizing Pattern

Droids **automatically discover and select** relevant skills based on task context:

```
User Request: "Implement user authentication"
     ↓
Orchestrator analyzes task
     ↓
Options:
  A) Simple task → Orchestrator uses skills directly
     - Invokes test-driven-development
     - Invokes verification-before-completion

  B) Complex task → Delegates to specialists
     - Tells backend-architect: "TDD skill available"
     - Backend-architect self-selects and uses TDD
```

**No decision matrix needed** - droids autonomously choose based on:
- Task type (implementing vs debugging vs planning)
- Current situation (stuck >10min → systematic-debugging)
- Discipline requirements (always verification-before-completion)

---

## 📖 Available Skills (20 Total)

### 🧪 Testing Skills (3)

**test-driven-development**
- **Use when:** Implementing new features, fixing bugs, refactoring
- **Provides:** Red-Green-Refactor methodology
- **Core principle:** Write failing tests before implementation

**condition-based-waiting**
- **Use when:** Tests have arbitrary delays or race conditions
- **Provides:** Active polling instead of sleep/setTimeout
- **Core principle:** Wait for actual conditions, not guessed timeouts

**testing-anti-patterns**
- **Use when:** Writing or modifying tests
- **Provides:** Avoidance of mock validation pitfalls
- **Core principle:** Test behavior, not mocks

### 🐛 Debugging Skills (4)

**systematic-debugging**
- **Use when:** Any bug, test failure, unexpected behavior
- **Provides:** 4-phase root cause methodology
- **Critical when:** Stuck >10min or after 2+ failed fix attempts

**root-cause-tracing**
- **Use when:** Errors occur deep in call stacks
- **Provides:** Backward tracing to data origin
- **Core principle:** Fix causes, not symptoms

**verification-before-completion**
- **Use when:** Before ANY completion claim
- **Provides:** Evidence-based validation
- **Critical rule:** Fresh command execution required before "done"

**defense-in-depth**
- **Use when:** Bugs manifest deep in workflows
- **Provides:** Multi-layer validation strategy
- **Core principle:** Validate at every layer

### 🤝 Collaboration Skills (9)

**brainstorming**
- **Use when:** Requirements are rough or unclear
- **Provides:** Ideas → designs via incremental questions
- **Core principle:** One question at a time, validate incrementally

**writing-plans**
- **Use when:** Design is finalized, ready for implementation
- **Provides:** Detailed task breakdown (2-5 min tasks with TDD)
- **Core principle:** Exact file paths, complete code examples

**executing-plans**
- **Use when:** Given complete implementation plan
- **Provides:** Batch execution with review checkpoints
- **Core principle:** Execute in batches, pause for feedback

**dispatching-parallel-agents**
- **Use when:** 3+ independent failures across subsystems
- **Provides:** Concurrent problem solving
- **Core principle:** One agent per independent domain

**requesting-code-review**
- **Use when:** After completing tasks or major features
- **Provides:** Quality gates via reviewer subagent
- **Core principle:** Review early, review often

**receiving-code-review**
- **Use when:** Processing reviewer feedback
- **Provides:** Technical evaluation and response
- **Core principle:** Verify feedback, don't blindly implement

**using-git-worktrees**
- **Use when:** Starting isolated feature work
- **Provides:** Safe parallel workspace management
- **Core principle:** Systematic directory selection + safety checks

**finishing-a-development-branch**
- **Use when:** Implementation complete, tests pass
- **Provides:** Verification and merge/PR/cleanup decisions
- **Core principle:** Verify first, then present options

**subagent-driven-development**
- **Use when:** Executing plans with quality gates
- **Provides:** Fresh subagent per task + review between tasks
- **Core principle:** High quality through continuous checkpoints

### 🔧 Meta Skills (4)

**writing-skills**
- **Use when:** Creating new skills
- **Provides:** TDD methodology for documentation
- **Core principle:** Watch agents fail first, then document

**sharing-skills**
- **Use when:** Contributing skills upstream
- **Provides:** Git workflow for skill PRs
- **Core principle:** Test before sharing

**testing-skills-with-subagents**
- **Use when:** Validating discipline-enforcing skills
- **Provides:** RED-GREEN-REFACTOR with pressure scenarios
- **Core principle:** Test under pressure, close loopholes

**using-superpowers**
- **Use when:** Start of ANY task
- **Provides:** Protocol to check available skills
- **Core principle:** If skill exists for task, must use it

---

## 🚀 How to Use Skills

### For Orchestrator

The orchestrator has **dual-use capability**:

**1. Self-Execution (Simple Tasks)**
```
User: "Fix bug in authentication"
Orchestrator:
  - Invokes systematic-debugging skill
  - Follows 4-phase root cause investigation
  - Uses verification-before-completion before claiming fixed
```

**2. Delegation (Complex Tasks)**
```
User: "Build complete authentication system"
Orchestrator:
  - Delegates to @backend-architect
  - Mentions: "Use test-driven-development skill"
  - Backend-architect self-selects and applies TDD
```

### For Individual Droids

All 104 droids are skill-aware:

**Discovery:**
- Skills located in `.factory/skills/`
- Listed in each droid's "Available Skills" section
- Self-discover based on task context

**Selection:**
- Review task requirements
- Check "When to Use Skills" guidance
- Choose relevant methodologies

**Invocation:**
- Use Skill tool: `invoke skill: test-driven-development`
- Follow skill instructions precisely
- Report skill usage in output

---

## 💡 Usage Examples

### Example 1: Feature Implementation

```
User Request: "Add password reset feature"

Orchestrator Decision:
  - Medium complexity → Delegate to backend-architect

Delegation Prompt:
  "@backend-architect: Implement password reset endpoints.
   Use test-driven-development skill for quality."

Backend-Architect Execution:
  1. Invokes test-driven-development skill
  2. Writes failing test for password reset
  3. Verifies test fails (RED)
  4. Implements minimal password reset logic
  5. Verifies test passes (GREEN)
  6. Refactors for clarity
  7. Uses verification-before-completion before reporting done
```

### Example 2: Bug Investigation

```
User Request: "Login fails with 500 error"

Orchestrator Decision:
  - Debugging task → Use systematic-debugging directly

Orchestrator Execution:
  1. Invokes systematic-debugging skill
  2. Phase 1: Root Cause Investigation
     - Reads error logs
     - Traces stack trace
     - Reproduces issue
  3. Phase 2: Pattern Analysis
     - Finds working login in tests
     - Compares working vs broken
  4. Phase 3: Hypothesis Testing
     - Forms hypothesis about database connection
     - Tests with minimal changes
  5. Phase 4: Implementation
     - Creates test proving bug
     - Implements fix
     - Uses verification-before-completion
```

### Example 3: Planning Session

```
User Request: "I want to add real-time notifications, but not sure how"

Orchestrator Decision:
  - Rough requirements → Use brainstorming skill

Orchestrator Execution:
  1. Invokes brainstorming skill
  2. Asks clarifying questions (one at a time):
     - "Which is more important: A) Speed or B) Flexibility?"
     - "What type: A) Push notifications B) In-app C) Both?"
     - "Expected volume: A) <100/day B) 100-1K C) >1K?"
  3. Explores 2-3 approaches with trade-offs
  4. Presents design incrementally with validation
  5. Once design approved:
     - Invokes writing-plans skill
     - Creates detailed implementation plan
     - Delegates to specialists with plan
```

---

## 🎓 Best Practices

### When to Invoke Skills

**Always:**
- `verification-before-completion` before claiming work done
- `test-driven-development` when implementing features
- `systematic-debugging` when stuck >10min

**Frequently:**
- `brainstorming` when requirements unclear
- `requesting-code-review` after major changes
- `writing-plans` for complex implementations

**As Needed:**
- `condition-based-waiting` for flaky tests
- `root-cause-tracing` for deep stack errors
- `dispatching-parallel-agents` for multiple failures

### Selection Criteria

**Discipline Skills** (TDD, verification):
- Use even when unnecessary
- Prevents bugs through systematic approach
- Required, not optional

**Debugging Skills**:
- Use when stuck >10min
- Use after 2+ failed fix attempts
- Saves time through systematic investigation

**Collaboration Skills**:
- Use for planning and review
- Use for coordination workflows
- Proven patterns work better than ad-hoc

**Meta Skills**:
- Use when creating/testing/sharing skills
- Maintain quality standards
- Build skill ecosystem

---

## 🔧 Technical Details

### Skills Location
```
.factory/skills/
├── test-driven-development/SKILL.md
├── systematic-debugging/SKILL.md
├── brainstorming/SKILL.md
└── ... (17 more skills)
```

### Skill Format
Each skill has:
- **YAML frontmatter** with name and description
- **Core Principle** in quotes
- **When to Use** section with specific scenarios
- **Methodology** with step-by-step instructions
- **Examples** showing real-world usage
- **Integration notes** with other skills

### Invocation Method
```
# In droid/orchestrator prompts
Use Skill tool to invoke methodologies when relevant

# Example
"Invoking test-driven-development skill for this feature..."
```

---

## 📈 Benefits

**Before Skills:**
- Inconsistent approaches to common problems
- Manual instruction needed for best practices
- Droids reinvent solutions

**After Skills:**
- 20 proven methodologies available
- Systematic approaches to challenges
- Self-organizing quality improvement
- Droids apply best practices automatically

**Measurable Improvements:**
- Fewer bugs through TDD discipline
- Faster debugging through systematic approach
- Better planning through structured brainstorming
- Higher quality through review workflows

---

## 🆘 Troubleshooting

### Skill Not Available
```
Error: Skill 'xyz' not found

Solution: Check .factory/skills/ for available skills
List in droid's "Available Skills" section
```

### Skill Invocation Failed
```
Error: Could not invoke skill

Solution:
1. Verify skill exists in .factory/skills/
2. Check SKILL.md has proper YAML frontmatter
3. Ensure Skill tool is available
```

### Droid Not Using Skills
```
Issue: Droid completed task without using relevant skill

Possible causes:
1. Skill awareness section missing (check droid file)
2. Task context unclear (provide more specific guidance)
3. Skill not obviously relevant (mention explicitly)

Solution: Explicitly mention skill in prompt
"Use systematic-debugging skill for this investigation"
```

---

## 📚 Further Reading

- **Skill Definitions:** `.factory/skills/*/SKILL.md`
- **Orchestrator Integration:** `droids/orchestrator.md` (line 211+)
- **Droid Integration:** Any `droids/*.md` (end of file)
- **Original Source:** [obra/superpowers](https://github.com/obra/superpowers)

---

## 🤝 Contributing

Want to add new skills?

1. **Create skill** using `writing-skills` methodology
2. **Test with subagents** using `testing-skills-with-subagents`
3. **Place in** `.factory/skills/new-skill-name/SKILL.md`
4. **Update** skill awareness sections if needed
5. **Share upstream** using `sharing-skills` if broadly applicable

---

*Skills system enables systematic, high-quality work through proven methodologies. Use them!*
