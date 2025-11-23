---
name: sharing-skills
description: Use when contributing tested skills upstream via Git workflow. Guides forking, branching, committing, and creating PRs for skills that apply broadly and have been validated through TDD process.
---

# Sharing Skills

## Core Principle

**"Share broadly applicable, thoroughly tested skills via proper Git workflow."**

This skill guides contributors through sharing locally-developed skills with upstream repositories using standard open-source contribution patterns.

## When to Use This Skill

**Share skills when they:**
- Apply broadly across projects (not project-specific)
- Represent patterns others would benefit from
- Have been thoroughly tested using **writing-skills** TDD process
- Follow established documentation guidelines
- Solve common problems or enforce best practices

**Keep skills private when they:**
- Solve organization or project-specific problems
- Remain experimental or unstable
- Contain sensitive information (credentials, internal processes)
- Address overly narrow or niche use cases
- Haven't been tested adequately

## The Six-Step Workflow

### Step 1: Sync with Upstream 🔄

**Ensure local main branch is current:**

```bash
# Add upstream remote (if not already added)
git remote add upstream https://github.com/original-owner/superpowers.git

# Fetch latest changes
git fetch upstream

# Switch to main branch
git checkout main

# Merge upstream changes
git merge upstream/main

# Push to your fork
git push origin main
```

### Step 2: Create Feature Branch 🌿

**Use naming convention: `add-[skillname]-skill`**

```bash
# Create and switch to feature branch
git checkout -b add-systematic-debugging-skill

# Verify branch
git branch --show-current
# add-systematic-debugging-skill
```

**Naming examples:**
- `add-systematic-debugging-skill`
- `add-defense-in-depth-skill`
- `add-brainstorming-skill`

### Step 3: Create/Edit Skill File 📝

**Create skill in appropriate directory:**

```bash
# Create skill directory
mkdir -p skills/systematic-debugging

# Create skill file
touch skills/systematic-debugging/SKILL.md
```

**Write skill following format:**
```markdown
---
name: systematic-debugging
description: Use when... [searchable description]
---

# Systematic Debugging

## Core Principle
[Key principle in quotes]

## When to Use This Skill
[Specific scenarios]

## [Methodology sections...]

[Rest of skill content...]
```

### Step 4: Commit with Details 💾

**Descriptive commit message including testing:**

```bash
git add skills/systematic-debugging/

git commit -m "$(cat <<'EOF'
feat: add systematic-debugging skill

Adds systematic 4-phase debugging methodology to prevent
symptom-masking and ensure root cause fixes.

Testing:
- Tested with 5 subagents across bug scenarios
- Without skill: 3/5 applied symptom fixes
- With skill: 5/5 found and fixed root causes
- Addresses rationalizations: guessing, multiple changes, symptom masking

Skill enforces:
- Root cause investigation before fixes
- Pattern analysis of working vs broken code
- Hypothesis testing with minimal changes
- Single root-cause fixes only

Related: Complements root-cause-tracing skill
EOF
)"
```

**Commit message should include:**
- Feature type (feat:, docs:, fix:)
- Brief summary (50 chars)
- Detailed description
- Testing methodology and results
- What skill enforces
- Related skills

### Step 5: Push Feature Branch ⬆️

**Push to YOUR fork (origin):**

```bash
# Push with upstream tracking
git push -u origin add-systematic-debugging-skill

# Verify pushed
git branch -vv
# * add-systematic-debugging-skill abc123 [origin/add-systematic-debugging-skill] feat: add...
```

### Step 6: Create Pull Request 📬

**Use GitHub CLI or web interface:**

```bash
# Using GitHub CLI
gh pr create \
  --repo original-owner/superpowers \
  --title "Add systematic-debugging skill" \
  --body "$(cat <<'EOF'
## Summary
Adds systematic 4-phase debugging methodology that ensures bugs are fixed at root cause rather than through symptom patches.

## Problem Solved
Agents often apply quick symptom fixes instead of investigating root causes, leading to recurring bugs and technical debt.

## Skill Overview
**Core Principle:** "ALWAYS find root cause before attempting fixes"

**Methodology:**
1. Root Cause Investigation - understand WHAT and WHERE
2. Pattern Analysis - understand WHY
3. Hypothesis Testing - prove what needs to change
4. Implementation - fix root cause permanently

## Testing Validation
Tested with 5 subagents across different bug scenarios:
- **Without skill:** 3/5 applied symptom fixes
- **With skill:** 5/5 found and fixed root causes

Pressure tested with time constraints, multiple failures, and architectural issues.

## Skill Characteristics
- Token count: ~1200 words
- Category: Debugging
- Frequency: High (use for any bug/failure)
- Complements: root-cause-tracing, verification-before-completion

## Checklist
- [x] Tested using writing-skills TDD process
- [x] Watched agents fail without skill
- [x] Documented failures and rationalizations
- [x] Verified skill makes agents comply
- [x] Description is searchable
- [x] Closed testing loopholes
- [x] Follows established skill format
- [x] No sensitive information included

## Related Skills
- root-cause-tracing: For errors deep in call stacks
- verification-before-completion: Before claiming fix complete
- test-driven-development: For creating tests proving fix

Ready for review!
EOF
)"
```

**PR should include:**
- Clear summary of what skill does
- Problem it solves
- Testing methodology and results
- Skill characteristics (token count, category, frequency)
- Completed checklist
- Related skills

## Critical Requirements

### Requirement #1: Skill Must Be Tested

```
❌ "I wrote a skill, let me share it"
✅ "I wrote a skill, tested it with subagents using writing-skills TDD, now sharing"
```

**Testing evidence required in PR:**
- Scenarios tested
- Failure modes without skill
- Success rates with skill
- Rationalizations addressed

### Requirement #2: One Skill Per PR

```
❌ PR with 5 different skills
✅ PR with 1 skill (allows individual review/iteration)

Why: Individual skills can be reviewed, iterated, and merged independently
```

### Requirement #3: No Sensitive Information

```
❌ Internal company processes
❌ Proprietary techniques
❌ Client-specific details
❌ Credentials or secrets

✅ General applicable patterns
✅ Open-source methodologies
✅ Universal best practices
```

### Requirement #4: Broadly Applicable

```
❌ "How to use our specific internal tool"
✅ "How to apply TDD systematically"

❌ "Our company's code review checklist"
✅ "Technical approach to receiving code review feedback"
```

## PR Review Process

### Expect Feedback

**Common review points:**
- Skill clarity and searchability
- Token efficiency
- Testing thoroughness
- Overlap with existing skills
- Formatting consistency
- Documentation completeness

### Iterate on Feedback

```bash
# Make requested changes
vim skills/systematic-debugging/SKILL.md

# Commit improvements
git add skills/systematic-debugging/SKILL.md
git commit -m "Address review feedback: clarify Phase 2 process"

# Push updates
git push origin add-systematic-debugging-skill

# PR automatically updates
```

### Use receiving-code-review Skill

Apply **receiving-code-review** skill principles:
- Verify feedback against skill purpose
- Ask for clarification if unclear
- Implement changes one at a time
- Pushback respectfully when needed
- Test after changes

## Integration with Other Skills

- **Requires writing-skills** to create tested skill first
- **Use receiving-code-review** to process PR feedback
- **Apply verification-before-completion** before submitting PR
- **Follows brainstorming** if skill concept needs refinement

## Success Criteria

You're sharing skills correctly when:
- ✅ Skill has been tested with subagents
- ✅ Testing evidence included in PR
- ✅ One skill per PR
- ✅ Descriptive commit messages
- ✅ Comprehensive PR description
- ✅ No sensitive information
- ✅ Broadly applicable pattern
- ✅ Follows established format
- ✅ Ready for community use
