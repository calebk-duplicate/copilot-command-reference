# GitHub Copilot Agents & Commands Usage Guide

## Overview

This guide explains how to use the GitHub Copilot agents and commands configured for the Barge Run puzzle generation project. These tools are designed to accelerate Phase 2 implementation and provide streamlined development workflow automation.

### What Are Copilot Agents?

Copilot agents are specialized AI assistants configured with specific context files and expertise areas. Each agent understands particular aspects of the Barge Run codebase and can help with targeted tasks.

**Configuration:** Agent definitions are in `.github/agents/*.agent.md` files following GitHub's best practices.

### What Are Copilot Commands?

Copilot commands are pre-configured prompts in `.github/copilot-commands.json` that invoke specific agents with well-defined tasks. They provide a quick way to perform common operations without writing detailed instructions.

## Agents Reference

### 🧩 @puzzle-generator

**Expertise**: Puzzle generation algorithms and implementation

**When to Use**:
- Implementing new generation algorithms
- Optimizing existing generators
- Debugging generation failures
- Tuning difficulty parameters
- Integrating solver feedback

**Context Files**:
- `src/logic/hybridGenerator.ts` - Main entry point routing by difficulty
- `src/logic/iterativeGenerator.ts` - Phase 1 iterative deepening
- `src/logic/patternGenerator.ts` - Pattern-based generation (experimental)
- `src/logic/generator.ts` - Original backwards-shuffle
- `ALTERNATIVE_ALGORITHMS.md` - Algorithm options and designs
- `PHASE1_IMPLEMENTATION.md` - Phase 1 results
- `GENERATOR_GUIDE.md` - Usage guide and metrics

**Defined in:** `.github/agents/puzzle-generator.agent.md`

**Example Usage**:
```
@puzzle-generator Help me optimize the iterative deepening algorithm to reduce generation time
```

### 🧪 @testing-validation

**Expertise**: Testing, metrics, and quality validation

**When to Use**:
- Writing test suites
- Analyzing success rates
- Measuring performance
- Validating puzzle quality
- Generating statistical reports

**Context Files**:
- `src/testPatternGenerator.ts` - Pattern generator tests
- `src/testGenerator.ts` - Original generator tests
- `src/testGeneratorV2.ts` - V2 generator tests
- `scripts/generateLevels.ts` - Level generation script
- `PHASE1_IMPLEMENTATION.md` - Phase 1 metrics
- `GENERATOR_GUIDE.md` - Target success rates

**Defined in:** `.github/agents/testing-validation.agent.md`

**Example Usage**:
```
@testing-validation Analyze the success rates from the latest test run and compare against Phase 1 targets
```

### 🔗 @solver-integration

**Expertise**: Solver integration and move count validation

**When to Use**:
- Optimizing solver performance
- Validating move counts
- Debugging solver integration
- Ensuring puzzle solvability
- Implementing solver caching

**Context Files**:
- `src/logic/solver.ts` - Puzzle solver implementation
- `src/logic/engine.ts` - Game engine
- `src/logic/iterativeGenerator.ts` - Uses solver for validation
- `src/logic/hybridGenerator.ts` - Routes to generators
- `src/logic/replayValidator.ts` - Solution replay validation

**Defined in:** `.github/agents/solver-integration.agent.md`

**Example Usage**:
```
@solver-integration The solver seems slow when validating hard puzzles. Can you optimize it?
```

### 🚀 @phase2-implementation

**Expertise**: Phase 2 constraint satisfaction implementation

**When to Use**:
- Planning Phase 2 architecture
- Implementing constraint satisfaction
- Creating forward-building algorithms
- Building dependency chains
- Improving Hard puzzle success rates

**Context Files**:
- `ALTERNATIVE_ALGORITHMS.md` - Phase 2 design details
- `PHASE1_IMPLEMENTATION.md` - Current baseline
- `src/logic/hybridGenerator.ts` - Integration point
- `src/logic/iterativeGenerator.ts` - Phase 1 reference
- `src/logic/solver.ts` - Solver for validation
- `GENERATOR_GUIDE.md` - Current metrics

**Defined in:** `.github/agents/phase2-implementation.agent.md`

**Example Usage**:
```
@phase2-implementation Design the constraint satisfaction system for Hard puzzles targeting 80-95% success
```

### 🎮 @gameplay-ui

**Expertise**: Game UI, player interactions, and gameplay mechanics

**When to Use**:
- Implementing game UI components
- Handling player input and vehicle movement
- Managing game state and win conditions
- Creating animations and visual feedback
- Optimizing gameplay performance
- Fixing gameplay bugs
- Improving user experience

**Context Files**:
- `src/App.tsx` - Main application component
- `src/components/GameBoard.tsx` - Game board and interactions
- `src/components/LevelSelector.tsx` - Level selection UI
- `src/components/VechicleItem.tsx` - Vehicle rendering
- `src/logic/engine.ts` - Movement and collision logic
- `src/logic/solver.ts` - Hint system integration
- `src/logic/replayValidator.ts` - Move validation
- `src/types.ts` - Type definitions
- `src/data/levels.ts` - Level data
- `src/data/difficulty.ts` - Difficulty configuration

**Defined in:** `.github/agents/gameplay-ui.agent.md`

**Example Usage**:
```
@gameplay-ui Add smooth animations when vehicles move on the board
@gameplay-ui Fix the bug where vehicles can overlap after undo
```

## Commands Reference

### Generation Commands

#### `/generate-easy`
Generate an Easy difficulty puzzle using backwards-shuffle algorithm.

**Agent**: @puzzle-generator

**Usage**:
```
/generate-easy
/generate-easy gridSize="5x5"
```

**Expected Output**: Easy puzzle (3-8 moves, 40-60% success rate)

---

#### `/generate-medium`
Generate a Medium difficulty puzzle using iterative deepening algorithm.

**Agent**: @puzzle-generator

**Usage**:
```
/generate-medium
/generate-medium targetMoves=15
```

**Expected Output**: Medium puzzle (12-20 moves, 70-85% success rate)

---

#### `/generate-hard`
Generate a Hard difficulty puzzle (Phase 2 will improve this).

**Agent**: @puzzle-generator

**Usage**:
```
/generate-hard
```

**Expected Output**: Hard puzzle (15-35 moves, <5% success rate currently)

---

#### `/generate-batch`
Generate multiple puzzles for testing purposes.

**Agent**: @puzzle-generator

**Usage**:
```
/generate-batch difficulty="Medium" count=10
/generate-batch difficulty="Easy" count=50
```

**Expected Output**: Batch generation statistics and results

---

### Testing Commands

#### `/test-generator`
Run test suite for a specific difficulty level.

**Agent**: @testing-validation

**Usage**:
```
/test-generator difficulty="Medium"
/test-generator difficulty="Hard" iterations=100
```

**Expected Output**: Success rates, generation times, quality metrics

---

#### `/test-all-difficulties`
Run comprehensive tests across all difficulty levels.

**Agent**: @testing-validation

**Usage**:
```
/test-all-difficulties
```

**Expected Output**: Complete report comparing all difficulties against targets

---

#### `/analyze-generation`
Analyze generation logs and calculate success rates.

**Agent**: @testing-validation

**Usage**:
```
/analyze-generation
/analyze-generation logFile="./generation.log"
```

**Expected Output**: Statistical analysis of generation performance

---

#### `/benchmark-performance`
Measure generation speed and efficiency.

**Agent**: @testing-validation

**Usage**:
```
/benchmark-performance
```

**Expected Output**: Performance metrics, optimization opportunities

---

### Development Commands

#### `/add-constraint-algorithm`
Start implementing Phase 2 constraint satisfaction algorithm.

**Agent**: @phase2-implementation

**Usage**:
```
/add-constraint-algorithm
```

**Expected Output**: New constraint-based generator implementation

---

#### `/optimize-obstacle-placement`
Improve obstacle placement logic for better puzzle quality.

**Agent**: @puzzle-generator

**Usage**:
```
/optimize-obstacle-placement generator="iterative"
```

**Expected Output**: Optimized obstacle placement logic

---

#### `/refactor-generator`
Refactor existing generator code for better maintainability.

**Agent**: @puzzle-generator

**Usage**:
```
/refactor-generator file="hybridGenerator.ts"
```

**Expected Output**: Refactored code with improved structure

---

#### `/add-generator-feature`
Add a new feature to the generator system.

**Agent**: @puzzle-generator

**Usage**:
```
/add-generator-feature feature="Add obstacle clustering for more interesting puzzles"
```

**Expected Output**: New feature implementation integrated with existing system

---

### Debugging Commands

#### `/debug-generation-failure`
Debug why a puzzle generation failed.

**Agent**: @puzzle-generator

**Usage**:
```
/debug-generation-failure difficulty="Hard"
/debug-generation-failure difficulty="Medium" context="Fails after 8 iterations"
```

**Expected Output**: Root cause analysis and suggested fixes

---

#### `/validate-solver-integration`
Ensure solver is correctly integrated with generators.

**Agent**: @solver-integration

**Usage**:
```
/validate-solver-integration
```

**Expected Output**: Integration validation report and optimization suggestions

---

#### `/analyze-puzzle-quality`
Analyze if generated puzzles meet quality standards.

**Agent**: @testing-validation

**Usage**:
```
/analyze-puzzle-quality
/analyze-puzzle-quality difficulty="Medium"
```

**Expected Output**: Quality assessment with recommendations

---

#### `/trace-generation-steps`
Trace through generation algorithm step by step.

**Agent**: @puzzle-generator

**Usage**:
```
/trace-generation-steps difficulty="Medium"
```

**Expected Output**: Detailed step-by-step execution trace

---

### Documentation Commands

#### `/update-generator-guide`
Update GENERATOR_GUIDE.md with latest statistics and approaches.

**Agent**: @testing-validation

**Usage**:
```
/update-generator-guide
```

**Expected Output**: Updated documentation with current metrics

---

#### `/document-algorithm`
Document a new algorithm approach in detail.

**Agent**: @puzzle-generator

**Usage**:
```
/document-algorithm algorithm="Constraint Satisfaction Forward Builder"
```

**Expected Output**: Comprehensive algorithm documentation

---

#### `/update-phase-docs`
Update phase implementation documentation.

**Agent**: @phase2-implementation

**Usage**:
```
/update-phase-docs phase="2"
```

**Expected Output**: Updated phase documentation with progress

---

#### `/generate-metrics-report`
Generate a report of current generation metrics.

**Agent**: @testing-validation

**Usage**:
```
/generate-metrics-report
/generate-metrics-report format="json"
```

**Expected Output**: Comprehensive metrics report

---

### Phase 2 Specific Commands

#### `/phase2-status`
Check Phase 2 implementation status and next steps.

**Agent**: @phase2-implementation

**Usage**:
```
/phase2-status
```

**Expected Output**: Status report and prioritized next steps

---

#### `/design-constraint-system`
Design the constraint satisfaction system for Phase 2.

**Agent**: @phase2-implementation

**Usage**:
```
/design-constraint-system
```

**Expected Output**: Detailed constraint system design document

---

#### `/implement-forward-builder`
Implement forward-building with constraints.

**Agent**: @phase2-implementation

**Usage**:
```
/implement-forward-builder
```

**Expected Output**: Forward-building generator implementation

---

#### `/test-hard-generation`
Specifically test and validate hard puzzle generation.

**Agent**: @testing-validation

**Usage**:
```
/test-hard-generation
/test-hard-generation iterations=200
```

**Expected Output**: Comprehensive Hard difficulty testing report

---

### Gameplay & UI Commands

#### `/implement-game-feature`
Add a new gameplay feature or UI enhancement.

**Agent**: @gameplay-ui

**Usage**:
```
/implement-game-feature feature="Add keyboard arrow key controls for vehicle movement"
/implement-game-feature feature="Show move count and optimal solution hint"
```

**Expected Output**: New gameplay feature implementation

---

#### `/fix-game-bug`
Debug and fix a gameplay or UI bug.

**Agent**: @gameplay-ui

**Usage**:
```
/fix-game-bug bug="Vehicles overlap after multiple undo operations"
/fix-game-bug bug="Win condition not triggering on level completion"
```

**Expected Output**: Bug fix with root cause analysis

---

#### `/improve-game-controls`
Enhance player controls and input handling.

**Agent**: @gameplay-ui

**Usage**:
```
/improve-game-controls
```

**Expected Output**: Improved control responsiveness and usability

---

#### `/add-game-animation`
Add animations or visual effects to enhance gameplay.

**Agent**: @gameplay-ui

**Usage**:
```
/add-game-animation animation="Smooth vehicle sliding with easing"
/add-game-animation animation="Victory celebration with confetti"
```

**Expected Output**: Animation implementation with visual feedback

---

#### `/optimize-game-performance`
Optimize gameplay rendering and performance.

**Agent**: @gameplay-ui

**Usage**:
```
/optimize-game-performance
```

**Expected Output**: Performance improvements with profiling results

---

#### `/redesign-game-ui`
Redesign or improve game UI components.

**Agent**: @gameplay-ui

**Usage**:
```
/redesign-game-ui
/redesign-game-ui component="LevelSelector"
```

**Expected Output**: Redesigned UI with improved aesthetics

---

#### `/test-gameplay`
Test gameplay mechanics and user interactions.

**Agent**: @gameplay-ui

**Usage**:
```
/test-gameplay
```

**Expected Output**: Test results with edge cases identified

---

#### `/add-game-hint-system`
Implement or improve the hint system.

**Agent**: @gameplay-ui

**Usage**:
```
/add-game-hint-system
```

**Expected Output**: Hint system implementation with solver integration

---

## Common Workflows

### Workflow 1: Implementing Phase 2

**Goal**: Implement constraint satisfaction for Hard difficulty puzzles

**Steps**:

1. **Check current status**
   ```
   /phase2-status
   ```

2. **Design the constraint system**
   ```
   /design-constraint-system
   ```

3. **Implement forward-builder**
   ```
   /implement-forward-builder
   ```

4. **Test Hard generation**
   ```
   /test-hard-generation iterations=100
   ```

5. **Validate solver integration**
   ```
   /validate-solver-integration
   ```

6. **Update documentation**
   ```
   /update-phase-docs phase="2"
   ```

### Workflow 2: Debugging Generation Failures

**Goal**: Identify and fix generation failures for a specific difficulty

**Steps**:

1. **Test the failing difficulty**
   ```
   /test-generator difficulty="Medium" iterations=50
   ```

2. **Trace through the generation process**
   ```
   /trace-generation-steps difficulty="Medium"
   ```

3. **Debug specific failure**
   ```
   /debug-generation-failure difficulty="Medium" context="[observed symptoms]"
   ```

4. **Validate solver if needed**
   ```
   /validate-solver-integration
   ```

5. **Re-test after fixes**
   ```
   /test-generator difficulty="Medium" iterations=50
   ```

### Workflow 3: Optimizing Performance

**Goal**: Improve generation speed and efficiency

**Steps**:

1. **Benchmark current performance**
   ```
   /benchmark-performance
   ```

2. **Optimize obstacle placement**
   ```
   /optimize-obstacle-placement generator="iterative"
   ```

3. **Validate solver integration**
   ```
   /validate-solver-integration
   ```

4. **Test all difficulties**
   ```
   /test-all-difficulties
   ```

5. **Generate metrics report**
   ```
   /generate-metrics-report
   ```

### Workflow 4: Adding New Features

**Goal**: Add a new feature to the generator system

**Steps**:

1. **Plan the feature**
   ```
   @puzzle-generator I want to add [feature description]. How should I approach this?
   ```

2. **Implement the feature**
   ```
   /add-generator-feature feature="[detailed description]"
   ```

3. **Test with all difficulties**
   ```
   /test-all-difficulties
   ```

4. **Analyze puzzle quality**
   ```
   /analyze-puzzle-quality
   ```

5. **Update documentation**
   ```
   /update-generator-guide
   ```

### Workflow 5: Improving Gameplay Experience

**Goal**: Enhance gameplay UI and player interactions

**Steps**:

1. **Test current gameplay**
   ```
   /test-gameplay
   ```

2. **Implement improvements**
   ```
   /improve-game-controls
   /add-game-animation animation="Smooth vehicle movement transitions"
   ```

3. **Optimize performance**
   ```
   /optimize-game-performance
   ```

4. **Add new features**
   ```
   /implement-game-feature feature="Show hint button with solver-based suggestions"
   /add-game-hint-system
   ```

5. **Test final result**
   ```
   /test-gameplay
   ```

## Best Practices

### When to Use Agents vs Commands

**Use Commands When**:
- You need to perform a well-defined, common task
- You want quick, standardized output
- You're following a documented workflow

**Use Agents Directly When**:
- You have a unique or complex question
- You need exploratory help or design advice
- The task requires context beyond what commands provide

### Combining Agents

You can leverage multiple agents in sequence:

```
@puzzle-generator Design a new algorithm for [task]
@testing-validation Create tests for that algorithm
@solver-integration Optimize solver calls for it
@phase2-implementation Integrate it into Phase 2 architecture
@gameplay-ui Create UI to showcase the new algorithm
```

### Iterative Development

Use the report-test-refine cycle:

1. Generate metrics report: `/generate-metrics-report`
2. Identify weak areas
3. Make improvements using appropriate commands
4. Test changes: `/test-all-difficulties`
5. Repeat

### Context Awareness

Agents have access to specific files. When asking questions, reference these files:

```
@puzzle-generator Looking at iterativeGenerator.ts, how can I reduce the number of solver calls?
```

## Troubleshooting

### Issue: Low Success Rates

**Symptoms**: Generation fails frequently for a difficulty level

**Solutions**:
1. Run `/analyze-generation` to identify patterns
2. Use `/debug-generation-failure difficulty="[level]"`
3. Check solver integration: `/validate-solver-integration`
4. Review algorithm: Ask `@puzzle-generator` for optimization suggestions

### Issue: Slow Generation

**Symptoms**: Puzzles take too long to generate

**Solutions**:
1. Run `/benchmark-performance` to identify bottlenecks
2. Use `/optimize-obstacle-placement generator="[type]"`
3. Ask `@solver-integration` about solver performance
4. Consider refactoring: `/refactor-generator file="[file]"`

### Issue: Poor Puzzle Quality

**Symptoms**: Puzzles are solvable but not interesting

**Solutions**:
1. Run `/analyze-puzzle-quality difficulty="[level]"`
2. Ask `@puzzle-generator` about quality improvements
3. Review patterns: Check `ALTERNATIVE_ALGORITHMS.md`
4. Consider adding features: `/add-generator-feature`

### Issue: Unsure About Phase 2

**Symptoms**: Don't know what to implement next

**Solutions**:
1. Check status: `/phase2-status`
2. Review design: Ask `@phase2-implementation` for roadmap
3. Read `ALTERNATIVE_ALGORITHMS.md` for context
4. Design system: `/design-constraint-system`

## Metrics Reference

### Target Success Rates (from PHASE1_IMPLEMENTATION.md)

| Difficulty | Target Success Rate | Current Performance |
|-----------|---------------------|---------------------|
| Easy      | 40-60%             | ✅ 40-60% (Backwards-shuffle) |
| Medium    | 70-85%             | ✅ 70-85% (Iterative Deepening - Phase 1) |
| Hard      | 80-95% (Phase 2)   | ❌ <5% (Backwards-shuffle - needs Phase 2) |

### Target Move Counts

| Difficulty | Target Moves | Grid Size |
|-----------|-------------|-----------|
| Easy      | 3-8         | 4x5 to 5x5 |
| Medium    | 12-20       | 5x7 to 7x7 |
| Hard      | 15-35       | 7x9 to 9x9 |

### Generation Performance Targets

| Difficulty | Target Time | Algorithm |
|-----------|------------|-----------|
| Easy      | 1-5 sec    | Backwards-shuffle |
| Medium    | 5-10 sec   | Iterative Deepening |
| Hard      | 5-15 sec*  | Constraint Satisfaction (Phase 2) |

*Phase 2 target; currently much slower with backwards-shuffle

## Quick Reference

### Most Used Commands

For daily development, these are the most useful commands:

```bash
# Generate puzzles
/generate-medium
/generate-batch difficulty="Medium" count=10

# Test and validate
/test-all-difficulties
/analyze-puzzle-quality

# Debug issues
/debug-generation-failure
/trace-generation-steps difficulty="Medium"

# Phase 2 work
/phase2-status
/test-hard-generation
```

### Key Files to Know

- **Algorithm Implementations**: `src/logic/hybridGenerator.ts`, `src/logic/iterativeGenerator.ts`
- **Testing**: `src/testPatternGenerator.ts`, `scripts/generateLevels.ts`
- **Documentation**: `PHASE1_IMPLEMENTATION.md`, `ALTERNATIVE_ALGORITHMS.md`, `GENERATOR_GUIDE.md`
- **Solver**: `src/logic/solver.ts`

## Getting Help

If you're unsure which agent or command to use:

1. **For generation problems**: Start with `@puzzle-generator`
2. **For testing/metrics**: Start with `@testing-validation`
3. **For solver issues**: Start with `@solver-integration`
4. **For Phase 2 work**: Start with `@phase2-implementation`
5. **For gameplay/UI issues**: Start with `@gameplay-ui`

You can also ask any agent:
```
@[agent-name] What can you help me with?
```

---

## Additional Resources

- **Phase 1 Results**: See `PHASE1_IMPLEMENTATION.md` for complete Phase 1 metrics
- **Algorithm Options**: See `ALTERNATIVE_ALGORITHMS.md` for detailed algorithm designs
- **Usage Guide**: See `GENERATOR_GUIDE.md` for current generator usage
- **Test Scripts**: Check `src/testPatternGenerator.ts` for testing examples

---

**Last Updated**: Migrated to individual agent files following GitHub best practices (January 2026)

**Agent Configuration**: Agents are now defined in `.github/agents/*.agent.md` files instead of the deprecated `copilot-agents.yml` format.

**Next Milestone**: Phase 2 - Constraint Satisfaction for Hard puzzles (Target: 80-95% success)
