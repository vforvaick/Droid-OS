# Init Orchestrator - Comprehensive Greenfield Project Initialization

## Overview

The **Init Orchestrator** (`/init-orchestrator`) is designed for **comprehensive greenfield project initialization** with full codebase analysis, intelligent tech stack detection, and coordinated multi-specialist execution.

## When to Use This Command

### ✅ Use `/init-orchestrator` for:

- **Greenfield projects**: Building entire applications from scratch
- **Large-scale features**: Multi-domain features requiring comprehensive planning
- **Project bootstrapping**: Setting up new projects with proper architecture
- **Complex migrations**: Refactoring or rebuilding major system components
- **Full system analysis**: When you need deep understanding of project requirements

**Example use cases:**
```bash
/init-orchestrator build a marketplace app with user authentication, product catalog, and payment processing
/init-orchestrator create a real-time chat application with mobile support
/init-orchestrator bootstrap a microservices architecture for e-commerce
```

### ❌ Use `/orchestrator` instead for:

- **Focused tasks**: Adding specific features to existing modules
- **Bug fixes**: Fixing issues in specific files
- **Simple modifications**: Updating configurations or styles
- **Incremental features**: Adding functionality to established projects

**Example use cases:**
```bash
/orchestrator add pagination to the users table component
/orchestrator fix the login validation bug in auth.service.ts
/orchestrator update the primary color in tailwind config
```

## Key Differences

| Feature | `/init-orchestrator` | `/orchestrator` |
|---------|---------------------|-----------------|
| **Scope** | Comprehensive project analysis | Minimal, task-focused scanning |
| **Scanning** | Full tech stack detection | Only mentioned files + dependencies |
| **Planning** | Multi-phase with all specialists | Focused plan for specific task |
| **Use Case** | Greenfield, large features | Focused tasks, bug fixes |
| **Complexity** | Handles 4+ domain specialists | Typically 1-2 specialists |
| **Speed** | Thorough but slower | Fast and focused |

## Configuration Schema

This file contains a JSON configuration that drives the Init Orchestrator behavior:

### Main Sections

1. **`context_detection`**: Auto-detects project characteristics
   - Scans for framework patterns (Next.js, Django, etc.)
   - Identifies tech stack from config files
   - Infers project structure and complexity

2. **`droid_selection`**: Intelligently selects specialist droids
   - Auto-ranks droids based on project needs
   - Uses historical success rates (learning weight: 0.3)
   - Provides fallback strategies when specialists unavailable

3. **`core_logic`**: Defines the execution workflow
   - Discover → Understand → Plan → Execute
   - Adaptive execution strategies (sequential/parallel/hybrid)
   - Proactive issue resolution for common patterns

4. **`learning`**: Continuous improvement system
   - Tracks success patterns across projects
   - Learns from failures to avoid repeating mistakes
   - Adapts prompts based on historical performance

5. **`prompt_enhancement`**: Context enrichment
   - Automatically includes best practices
   - Injects security and performance guidelines
   - Adds framework-specific recommendations

6. **`execution_flow`**: Step-by-step process
   - 7-step comprehensive workflow
   - Factory-managed parallel specialist execution
   - Autonomous continuation until completion

## How to Adapt This Configuration

### For Specific Project Types

**Frontend-Heavy Projects:**
- Increase weight for `frontend-developer`, `ui-ux-designer`
- Add framework-specific scan patterns (e.g., `vite.config.js`, `angular.json`)

**Backend-Heavy Projects:**
- Prioritize `backend-architect`, `database-admin`
- Add API-specific patterns (e.g., `swagger.yaml`, `graphql.schema`)

**Microservices:**
- Enable `devops-specialist`, `kubernetes-architect`
- Add container patterns (e.g., `docker-compose.yml`, `k8s/*.yaml`)

### Modifying Execution Strategy

Edit `core_logic.execution_strategy.adaptation_rules`:
```json
"adaptation_rules": {
  "high_complexity": "sequential_with_checkpoints",  // More control
  "low_risk": "parallel_execution",                  // Faster
  "mixed_dependencies": "hybrid_strategy"            // Balanced
}
```

### Adjusting Learning Weight

Modify `droid_selection.learning_weight` (0.0 to 1.0):
- **0.0**: Pure pattern matching, no historical learning
- **0.3** (default): Balanced - 70% current match, 30% history
- **1.0**: Full reliance on historical success rates

## Memory System Integration

The Init Orchestrator leverages persistent memory for continuous improvement:

- **Success Patterns**: `$HOME/.factory/orchestrator/memory/success_patterns.json`
- **Failure Patterns**: `$HOME/.factory/orchestrator/memory/failure_patterns.json`
- **Project Templates**: `$HOME/.factory/orchestrator/memory/project_templates.json`
- **Learning Metrics**: `$HOME/.factory/orchestrator/memory/learning_metrics.json`

These files are automatically read during initialization and updated after project completion.

## Example Workflow

When you run `/init-orchestrator build a SaaS application`:

1. **Discovery**: Scans for existing files, detects tech stack
2. **Understanding**: Analyzes requirements, identifies domains (auth, billing, frontend)
3. **Planning**: Creates comprehensive multi-phase plan with specialist assignments
4. **Execution**: Coordinates 4+ specialists in parallel/sequential phases:
   - Backend Architect: API design, database schema
   - Frontend Developer: UI components, user flows
   - Security Auditor: Auth flows, data protection
   - Payment Integration: Billing system, subscription management
5. **Synthesis**: Integrates all work into cohesive application
6. **Learning**: Updates memory with patterns that worked

---

## Configuration JSON

The configuration below defines the complete behavior of the Init Orchestrator:

```json
{
  "command": "orchestrator",
  "action": "invoke_droid",
  "droid": "@orchestrator",
  "objective": "Execute entire project using intelligent planning, adaptive execution, and Factory-managed parallel specialist execution",
  "system_structure": {
    "droids_location": "/droids/",
    "droids_purpose": "Specialist agents for implementation",
    "orchestrator_location": "/orchestrator/",
    "orchestrator_contents": ["configuration", "task-patterns.json", "coordination logic"],
    "working_directory": "current project location"
  },
  "context_detection": {
    "scan_patterns": [
      "package.json:frontend_frameworks",
      "requirements.txt:python_frameworks",
      "Dockerfile:containerization",
      "*.sql:database_schema",
      "src/:project_structure",
      "*.md:documentation",
      "tsconfig.json:typescript_config",
      "next.config.js:nextjs_config",
      "composer.json:php_config"
    ],
    "auto_detect": true,
    "tech_stack_inference": true
  },
  "droid_selection": {
    "auto_rank": true,
    "criteria": [
      "project_complexity",
      "tech_stack_match",
      "dependency_analysis",
      "expertise_level",
      "success_history"
    ],
    "fallback_strategies": [
      "use_similar_droid",
      "use_generalist_droid",
      "ask_user_preference"
    ],
    "learning_weight": 0.3
  },
  "core_logic": {
    "1_discover": {
      "action": "Discover project context organically",
      "common_files_to_check": ["README.md", "package.json", "requirements.txt", "plan.md", "*.md", "docs/", "src/"],
      "method": "Targeted file reading, no directory listing"
    },
    "2_understand": {
      "action": "Analyze existing project files and user request",
      "goal": "Determine project scope and requirements"
    },
    "3_plan": {
      "action": "Create phased execution approach",
      "use": ["orchestrator patterns from /orchestrator/", "available specialist droids from /droids/"]
    },
    "4_execute": {
      "action": "Request Factory to execute specialist droids in parallel with detailed prompts",
      "mode": "Factory-managed parallel execution with orchestrator coordination"
    },
    "execution_strategy": {
      "auto_optimize": true,
      "factors": [
        "dependency_graph",
        "parallel_potential",
        "risk_assessment",
        "resource_availability"
      ],
      "adaptation_rules": {
        "high_complexity": "sequential_with_checkpoints",
        "low_risk": "parallel_execution",
        "mixed_dependencies": "hybrid_strategy"
      }
    },
    "proactive_resolution": {
      "predict_issues": true,
      "preemptive_solutions": true,
      "risk_mitigation": true,
      "contingency_planning": true,
      "common_patterns": {
        "authentication": ["security_first", "test_early"],
        "payment_systems": ["pci_compliance", "multiple_gateways"],
        "real_time_features": ["scaling_strategy", "connection_pooling"],
        "database_heavy": ["performance_optimization", "query_optimization"]
      }
    },
    "learning": {
      "track_success_patterns": true,
      "learn_from_failures": true,
      "adapt_prompts": true,
      "memory_retention": "project_history",
      "cross_project_learning": {
        "pattern_recognition": true,
        "success_factor_analysis": true,
        "template_generation": true
      }
    },
    "monitoring": {
      "progress_tracking": true,
      "bottleneck_detection": true,
      "performance_metrics": true,
      "quality_gates": true,
      "adaptive_scheduling": {
        "reorder_tasks": true,
        "parallel_when_possible": true,
        "optimize_wait_times": true
      }
    }
  },
  "available_droids": {
    "backend": ["backend-architect", "backend-typescript-architect", "backend-security-coder", "database-admin", "database-architect", "database-optimizer"],
    "frontend": ["frontend-developer", "frontend-security-coder", "nextjs-app-router-developer", "ui-ux-designer"],
    "mobile": ["mobile-developer", "mobile-security-coder", "flutter-expert"],
    "security": ["security-auditor", "blue-team-specialist", "red-team-specialist", "incident-responder"],
    "testing": ["test-automator", "tdd-orchestrator", "code-reviewer", "debugger", "error-detective"],
    "devops": ["devops-specialist", "devops-troubleshooter", "cloud-architect", "kubernetes-architect", "terraform-specialist"],
    "data": ["data-engineer", "data-analyst", "data-scientist", "mlops-engineer"],
    "documentation": ["documentation-specialist", "docs-architect", "api-documenter", "tutorial-engineer"],
    "performance": ["performance-engineer", "observability-engineer"],
    "specialized": ["payment-integration", "blockchain-developer", "ai-engineer", "ml-engineer"],
    "total": "104 droids available in /droids/"
  },
  "prompt_enhancement": {
    "auto_context": true,
    "include_patterns": [
      "similar_projects",
      "best_practices",
      "security_considerations",
      "performance_guidelines",
      "industry_standards",
      "framework_specifics"
    ],
    "dynamic_injection": {
      "current_project_context": true,
      "detected_tech_stack": true,
      "dependency_analysis": true,
      "risk_assessment": true
    }
  },
  "phase_planning": {
    "auto_generate": true,
    "adaptive_phases": true,
    "milestone_detection": true,
    "checkpoint_system": true,
    "smart_dependencies": {
      "auto_detect": true,
      "circular_dependency_detection": true,
      "optimization_suggestions": true
    }
  },
  "error_recovery": {
    "auto_diagnose": true,
    "alternative_strategies": true,
    "graceful_degradation": true,
    "rollback_capabilities": true,
    "resilience_patterns": {
      "retry_with_different_approach": true,
      "fallback_to_simpler_solution": true,
      "escalation_when_needed": true,
      "learn_from_failures": true
    }
  },
  "knowledge_base": {
    "store_solutions": true,
    "reuse_patterns": true,
    "avoid_mistakes": true,
    "success_templates": true,
    "failure_patterns": true
  },
  "execution_flow": [
    {
      "step": 1,
      "action": "Analyze current directory for project context",
      "method": "Targeted file reading, no directory listing",
      "forbidden": ["listing .factory directory", "reading /droids/orchestrator.md"]
    },
    {
      "step": 2,
      "action": "Read orchestrator configuration",
      "source": "/orchestrator/",
      "files": ["task-patterns.json", "orchestrator-config.json"]
    },
    {
      "step": 3,
      "action": "Create TodoWrite with comprehensive phase breakdown",
      "include": "All phases and sub-tasks"
    },
    {
      "step": 4,
      "action": "Execute through layers",
      "layers": ["Discovery", "Planning", "Implementation", "Review"],
      "sequential": true
    },
    {
      "step": 5,
      "action": "Request Factory parallel execution",
      "method": "Generate specialist prompts and delegate to Factory for parallel droid execution"
    },
    {
      "step": 6,
      "action": "Synthesize results and ensure integration",
      "verify": "All components work together"
    },
    {
      "step": 7,
      "action": "Continue autonomously until project completion",
      "mode": "autonomous"
    }
  ],
  "key_principles": [
    "Use context-aware discovery, not hardcoded assumptions",
    "Find project structure and requirements organically",
    "No unnecessary directory listing",
    "Targeted file reading based on common patterns",
    "Continuous autonomous execution"
  ]
}
```
