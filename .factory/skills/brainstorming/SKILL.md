---
name: brainstorming
description: Use before writing code when requirements are rough or unclear. Transforms ideas into fully-formed designs through incremental questions, exploration of alternatives, and validation checkpoints.
---

# Brainstorming

## Core Principle

**"One question at a time, multiple-choice when possible, validate incrementally."**

This skill transforms rough ideas into fully-formed designs through structured dialogue that emphasizes asking clarifying questions before proposing solutions.

## When to Use This Skill

**Deploy this skill:**
- **Before writing code or implementation plans**
- During creative development phases
- When user provides rough or unclear requirements
- When exploring solution spaces
- When need to refine concepts collaboratively

**Avoid using for:**
- Clear "mechanical" processes where the path is established
- Implementation of already-finalized designs
- Simple, well-defined tasks with obvious solutions

## The Three-Phase Methodology

### Phase 1: Understanding the Idea 🔍

**Goal:** Refine rough concepts into clear requirements

**Process:**
1. **Review project context first** - Understand existing codebase, patterns, constraints
2. **Ask questions one at a time** - Don't overwhelm with multiple questions
3. **Prioritize multiple-choice questions** - Easier to answer than open-ended
4. **Focus on discovery** - Purpose, constraints, success criteria

**Example Questions:**
```
❌ "Tell me everything about what you want to build"
✅ "Which is more important for this feature:
   A) Speed of implementation
   B) Flexibility for future changes
   C) Minimal code complexity"
```

**Key Principles:**
- One question at a time (avoid overwhelming)
- Multiple-choice over open-ended (when feasible)
- Clarify assumptions before proceeding
- Understand "why" not just "what"

### Phase 2: Exploring Approaches 🗺️

**Goal:** Present options with trade-offs, not prescriptions

**Process:**
1. **Present 2-3 different approaches** with clear trade-offs
2. **Lead with recommended approach** and reasoning
3. **Discuss options conversationally** rather than prescriptively
4. **Explore implications** of each choice

**Example Exploration:**
```markdown
I see three ways to implement this:

**Option A: Server-side rendering (Recommended)**
- Pros: Better SEO, faster initial load, simpler client
- Cons: More server load, less interactive
- Best when: SEO matters, content-heavy pages

**Option B: Client-side rendering**
- Pros: Highly interactive, less server load
- Cons: Slower initial load, SEO challenges
- Best when: App-like experience, authenticated users

**Option C: Hybrid approach**
- Pros: Best of both, flexible
- Cons: More complex, harder to maintain
- Best when: Mixed content types, large application

I lean toward Option A because [reasoning]. What do you think?
```

**Key Principles:**
- Always present trade-offs, not just benefits
- Lead with recommendation + reasoning
- Make it conversational, not directive
- Allow space for pushback

### Phase 3: Design Presentation 📐

**Goal:** Present complete design with validation checkpoints

**Process:**
1. **Break into 200-300 word sections** - Digestible chunks
2. **Ask after each section** - "Does this look right so far?"
3. **Cover all aspects systematically:**
   - Architecture overview
   - Component breakdown
   - Data flow and state management
   - Error handling strategy
   - Testing approach

**Example Section:**
```markdown
## Architecture Overview

I'm proposing a three-tier architecture:
- **API Layer**: RESTful endpoints handling auth + data
- **Business Logic**: Service classes for order processing
- **Data Layer**: PostgreSQL with Prisma ORM

The data flows: Client → API → Service → Database → Service → API → Client

Does this high-level structure make sense before I go into details?

[Wait for confirmation before continuing...]
```

**Key Principles:**
- Small sections with checkpoints
- Validate incrementally, not at the end
- Allow course corrections early
- Maintain flexibility for clarification

## Special Techniques

### Technique #1: YAGNI Enforcement

**"You Aren't Gonna Need It"** - Remove unnecessary features aggressively

```
User: "Let's add OAuth, email login, and phone authentication"
You: "For this MVP, which ONE authentication method is most critical?
     We can add others later if needed."
```

### Technique #2: Constraint Surfacing

Ask about constraints explicitly:

```
Multiple Choice:
"What's your main constraint for this project:
 A) Time - need it shipped fast
 B) Budget - minimize costs
 C) Quality - must be production-grade
 D) Learning - want to try new tech"
```

### Technique #3: Assumption Validation

State assumptions explicitly:

```
"Based on our discussion, I'm assuming:
 - You have an existing Next.js project
 - You're comfortable with TypeScript
 - Database is already set up

Is this correct, or should I adjust my approach?"
```

### Technique #4: Risk Identification

Surface potential issues early:

```
"I see a few potential challenges:
 1. Real-time updates might need WebSocket infrastructure
 2. Payment processing requires PCI compliance
 3. Internationalization adds significant complexity

Should we address these now, or defer for later?"
```

## Post-Brainstorming Actions

### Action #1: Document the Design

Save the validated design:
```
docs/plans/YYYY-MM-DD-<topic>-design.md
```

### Action #2: Commit to Git

Commit the design document:
```bash
git add docs/plans/
git commit -m "docs: brainstormed design for [feature]"
```

### Action #3: Transition to Implementation

**Choose next step:**
- Use **writing-plans** skill to create detailed implementation plan
- Use **using-git-worktrees** to create isolated workspace
- Begin implementation directly if design is simple

## Examples

### Example 1: Feature Brainstorming

```
User: "I want to add a chat feature"

You: "Let me understand the requirements. Which is most important:
     A) Real-time messaging (like Slack)
     B) Email-style messaging (like inbox)
     C) Simple comment threads (like GitHub issues)"

User: "B - email-style"

You: "Got it. One more question - authentication:
     A) Add to existing user system
     B) Create separate chat accounts
     C) Allow anonymous messaging"

User: "A - existing users"

You: "Perfect. Here's my recommended approach:
     [Present design incrementally with checkpoints...]"
```

### Example 2: Architecture Brainstorming

```
User: "We need better performance"

You: "Let's identify the bottleneck first. Where are users experiencing slowness:
     A) Initial page load
     B) Interactions after page loads
     C) Specific features (which ones?)
     D) Not sure - need profiling"

User: "A - initial page load"

You: "For initial load performance, I see three approaches:
     [Present options with trade-offs...]"
```

## Red Flags

### 🚩 Red Flag #1: Jumping to Solutions

```
❌ User: "I want better UX"
    You: "Let's add animations and micro-interactions!"

✅ User: "I want better UX"
    You: "What specific UX problem are users experiencing?"
```

### 🚩 Red Flag #2: Multiple Questions at Once

```
❌ "What tech stack do you want, who are the users, what's the timeline,
    what's the budget, and what features are priority?"

✅ "Let's start with the most important decision - what's your main
    constraint: time, budget, or quality?"
```

### 🚩 Red Flag #3: No Trade-off Discussion

```
❌ "We should use microservices - they're scalable!"

✅ "Microservices offer scalability but add complexity. For your current
    scale, a monolith might be simpler. What matters more right now?"
```

## Integration with Other Skills

- **Follow with writing-plans** to create detailed implementation
- **Use using-git-worktrees** to create isolated development environment
- **Apply test-driven-development** during implementation
- **Use requesting-code-review** after implementation

## Success Criteria

You're brainstorming effectively when:
- ✅ Asking one clear question at a time
- ✅ Using multiple-choice questions frequently
- ✅ Validating each design section before continuing
- ✅ Presenting options with honest trade-offs
- ✅ Removing unnecessary features (YAGNI)
- ✅ User confirms understanding at each checkpoint
- ✅ Design is actionable and complete before implementation
