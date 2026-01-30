# .github Directory

This directory contains GitHub-specific configuration files and documentation for the Barge Run puzzle generation project.

## Contents

### Configuration Files

#### Agent Definitions (`.github/agents/*.agent.md`)
Defines 5 specialized GitHub Copilot agents for the project (following 2026 best practices):
- **@puzzle-generator**: Expert in puzzle generation algorithms
- **@testing-validation**: Expert in testing and quality metrics
- **@solver-integration**: Expert in solver integration and validation
- **@phase2-implementation**: Specialist for Phase 2 constraint satisfaction
- **@gameplay-ui**: Expert in game UI, player interactions, and gameplay mechanics

Each agent is defined in a separate `.agent.md` file with YAML frontmatter and detailed instructions.

#### `copilot-commands.json`
Defines 32 pre-configured commands that invoke the agents with specific tasks:
- **Generation Commands** (4): Generate puzzles of various difficulties
- **Testing Commands** (4): Test and validate puzzle generation
- **Development Commands** (4): Implement and refactor features
- **Debugging Commands** (4): Debug and diagnose issues
- **Documentation Commands** (4): Update and maintain documentation
- **Phase 2 Commands** (4): Work on Phase 2 constraint satisfaction
- **Gameplay & UI Commands** (8): Enhance gameplay experience and UI

#### `copilot-instructions.md`
General GitHub Copilot instructions for the entire project:
- **Project Overview**: Architecture and component descriptions
- **Coding Standards**: TypeScript, React, and puzzle generation conventions
- **Testing Requirements**: Success rate targets and validation patterns
- **Key Patterns**: Generator patterns, solver usage, state management
- **Common Tasks**: Adding generators, game features, debugging workflows
- **Performance Considerations**: Optimization guidelines
- **Current Metrics**: Phase 1 success rates and benchmarks

This file is automatically read by GitHub Copilot to understand project-specific context and conventions.

### Documentation

#### `AGENT_USAGE_GUIDE.md`
Comprehensive guide for using the Copilot agents and commands:
- Detailed agent descriptions and use cases
- Complete command reference with examples
- Common workflow patterns
- Best practices and troubleshooting
- Metrics reference from Phase 1 implementation

## Quick Start

### Using Agents

Invoke an agent by mentioning it with the `@` symbol:

```
@puzzle-generator Help me optimize the iterative deepening algorithm
@testing-validation Analyze success rates for Medium difficulty
@solver-integration Debug solver performance issues
@phase2-implementation Design the constraint satisfaction system
@gameplay-ui Add smooth animations when vehicles move
```

### Using Commands

Use commands with the `/` prefix:

```
/generate-medium
/test-all-difficulties
/debug-generation-failure difficulty="Hard"
/phase2-status
/implement-game-feature feature="Add keyboard controls"
/add-game-animation animation="Vehicle movement transitions"
```

See `AGENT_USAGE_GUIDE.md` for complete documentation.

## Project Context

This configuration is designed to support the Barge Run puzzle generation system, which includes:

### Phase 1 (Complete) ✅
- Backwards-shuffle for Easy puzzles (40-60% success)
- Iterative deepening for Medium puzzles (70-85% success)
- Hybrid generator architecture routing by difficulty

### Phase 2 (In Progress) 🚧
- Constraint satisfaction for Hard puzzles (target: 80-95% success)
- Forward-building algorithm with dependency chains
- Enhanced puzzle quality and variety

## Key Files Referenced

The agents have access to these context files:

**Generation**:
- `src/logic/hybridGenerator.ts` - Main routing logic
- `src/logic/iterativeGenerator.ts` - Phase 1 iterative deepening
- `src/logic/patternGenerator.ts` - Experimental pattern-based
- `src/logic/generator.ts` - Original backwards-shuffle

**Testing**:
- `src/testPatternGenerator.ts` - Pattern tests
- `scripts/generateLevels.ts` - Level generation script

**Solver**:
- `src/logic/solver.ts` - Puzzle solver
- `src/logic/engine.ts` - Game engine
- `src/logic/replayValidator.ts` - Solution validation

**Documentation**:
- `PHASE1_IMPLEMENTATION.md` - Phase 1 results and metrics
- `ALTERNATIVE_ALGORITHMS.md` - Algorithm designs and Phase 2 plan
- `GENERATOR_GUIDE.md` - Usage guide and current metrics

## Target Metrics

| Difficulty | Success Rate | Move Count | Algorithm |
|-----------|-------------|------------|-----------|
| Easy      | 40-60%      | 3-8        | Backwards-shuffle |
| Medium    | 70-85% ✅   | 12-20      | Iterative Deepening |
| Hard      | 80-95%* 🚧  | 15-35      | Constraint Satisfaction (Phase 2) |

*Phase 2 target; currently <5% with backwards-shuffle

## Contributing

When adding new agents or commands:

1. Create a new `.agent.md` file in `.github/agents/` with YAML frontmatter
2. Add corresponding commands to `copilot-commands.json`
3. Document usage in `AGENT_USAGE_GUIDE.md`
4. Update this README if adding new categories

## Support

For questions about using these tools, see:
- `AGENT_USAGE_GUIDE.md` - Complete usage documentation
- `PHASE1_IMPLEMENTATION.md` - Current implementation status
- `ALTERNATIVE_ALGORITHMS.md` - Algorithm design details

## Related Documentation

- **Project Root**: `/README.md` - Main project documentation
- **Generator Guide**: `/GENERATOR_GUIDE.md` - Generator usage and metrics
- **Phase 1 Results**: `/PHASE1_IMPLEMENTATION.md` - Phase 1 implementation details
- **Algorithm Designs**: `/ALTERNATIVE_ALGORITHMS.md` - Detailed algorithm documentation

---

*These configurations are designed to accelerate Phase 2 implementation and provide streamlined development workflow automation.*
