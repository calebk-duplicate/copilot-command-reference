# GitHub Copilot Instructions for Barge Run

## Quick Start Commands

**Build & Run:**
```bash
npm run dev           # Start development server
npm run build         # Build for production
npm run preview       # Preview production build
npm run generate-levels  # Generate puzzle levels
```

**Testing:**
```bash
tsx testPatternGenerator.ts    # Test pattern generator
tsx testEasyMedium.ts          # Test Easy/Medium generation
tsx testHardGeneration.ts      # Test Hard generation
```

## Project Overview

Barge Run is a puzzle game similar to Rush Hour where the player must navigate a vehicle (the player) to a goal position on a grid by moving vehicles that block the path. This project includes both the game implementation and a sophisticated puzzle generation system.

**Tech Stack:** React + TypeScript + Vite + Framer Motion

## Architecture

### Frontend (React + TypeScript)
- **App.tsx**: Main application component with level selection and game view
- **components/GameBoard.tsx**: Game board rendering and interaction handling
- **components/LevelSelector.tsx**: Level selection interface
- **components/VechicleItem.tsx**: Individual vehicle rendering with drag-and-drop

### Core Game Logic
- **src/logic/engine.ts**: Movement bounds calculation and collision detection
- **src/logic/solver.ts**: BFS-based puzzle solver for validation and hints
- **src/logic/replayValidator.ts**: Solution replay and validation

### Puzzle Generation System

#### Current Architecture (Phase 1 Complete)
The project uses a **hybrid generator** approach:

1. **Hybrid Generator** (`src/logic/hybridGenerator.ts`):
   - Routes to appropriate algorithm based on difficulty
   - Easy: Backwards-shuffle algorithm (40-60% success rate)
   - Medium: Iterative deepening algorithm (70-85% success rate)
   - Hard: Currently backwards-shuffle (<5% success rate)

2. **Iterative Deepening Generator** (`src/logic/iterativeGenerator.ts`):
   - Phase 1 implementation for Medium difficulty
   - Places player far from goal
   - Iteratively adds obstacles that increase move count
   - Validates each addition with solver
   - Typically completes in <10 iterations

3. **Pattern Generator** (`src/logic/patternGenerator.ts`):
   - Experimental pattern-based approach
   - Not currently used in production

#### Phase 2 Roadmap (In Progress)
- Implement constraint satisfaction for Hard difficulty
- Target: 80-95% success rate
- Forward-building with explicit move count constraints
- See `ALTERNATIVE_ALGORITHMS.md` for detailed design

## Coding Standards

### TypeScript
- Use TypeScript for all new code
- Define proper interfaces and types in `src/types.ts`
- Avoid `any` types; use proper type annotations
- Use strict mode settings

### React Components
- Use functional components with hooks (useState, useEffect, etc.)
- Keep components focused and single-purpose
- Use framer-motion for animations
- Follow existing naming conventions (PascalCase for components)

### Puzzle Generation
- Always validate puzzles are solvable using the solver
- Log generation statistics (attempts, time, move count)
- Follow difficulty targets:
  - Easy: 3-8 moves, grid 4x5 to 5x5
  - Medium: 12-20 moves, grid 5x7 to 7x7
  - Hard: 15-35 moves, grid 7x9 to 9x9
- Use existing generator patterns from hybrid architecture

### Game Logic
- Use the `engine.ts` movement bounds calculation
- Validate all moves with collision detection
- Maintain immutable state for undo/redo functionality
- Use the solver for hints and validation

## Testing Requirements

### Puzzle Generation Testing
- Test success rates match targets (see `PHASE1_IMPLEMENTATION.md`)
- Validate all generated puzzles are solvable
- Measure generation time and performance
- Use `src/testPatternGenerator.ts` as reference

### Gameplay Testing
- Test vehicle movement and collision detection
- Verify win conditions trigger correctly
- Test undo/redo functionality
- Validate level progression

## Key Patterns

### Generator Pattern
```typescript
function generateLevel(config: GeneratorConfig): Level | null {
  // 1. Create initial state
  // 2. Iteratively modify to reach target difficulty
  // 3. Validate with solver
  // 4. Return level or null if failed
}
```

### Solver Usage
```typescript
const solution = solvePuzzle(vehicles, config);
if (!solution || solution.length < targetMin || solution.length > targetMax) {
  return null; // Invalid puzzle
}
```

### React State Management
```typescript
const [vehicles, setVehicles] = useState<Vehicle[]>(level.vehicles);
// Use functional updates for state based on previous state
setVehicles(prev => prev.map(v => v.id === active.id ? updated : v));
```

## File Organization

### Source Structure
```
src/
├── components/        # React components
├── logic/            # Game and generation logic
├── data/             # Static data (levels, difficulty configs)
└── types.ts          # TypeScript type definitions
```

### Documentation
- `PHASE1_IMPLEMENTATION.md` - Phase 1 results and metrics
- `ALTERNATIVE_ALGORITHMS.md` - Algorithm designs and Phase 2 plan
- `GENERATOR_GUIDE.md` - Usage guide and current metrics
- `.github/AGENT_USAGE_GUIDE.md` - Copilot agents usage

## Common Tasks

### Adding a New Generator Algorithm
1. Create new file in `src/logic/` (e.g., `constraintGenerator.ts`)
2. Follow signature: `export function generateWithConstraints(config: GeneratorConfig): Level | null`
3. Validate puzzles with solver
4. Update `hybridGenerator.ts` to route to new algorithm
5. Add tests in `src/testPatternGenerator.ts`
6. Document in `ALTERNATIVE_ALGORITHMS.md`

### Adding a New Game Feature
1. Update relevant component in `src/components/`
2. Modify game state management if needed
3. Update `engine.ts` if logic changes required
4. Test with different difficulty levels
5. Ensure undo/redo still works

### Debugging Generation Failures
1. Check solver capacity (currently 50,000 states)
2. Verify grid size and obstacle counts are reasonable
3. Log intermediate states to trace algorithm progress
4. Use `testPatternGenerator.ts` to run batch tests
5. Compare against success rate targets in `PHASE1_IMPLEMENTATION.md`

## Performance Considerations

### Puzzle Generation
- Solver is expensive; minimize calls
- Cache solver results when possible
- Target generation time: <10 seconds per puzzle
- Use iterative approaches over brute force

### Gameplay Rendering
- Use React.memo for expensive components
- Minimize re-renders with proper dependency arrays
- Use framer-motion for smooth animations
- Keep 60fps during interactions

## Documentation Standards

### Code Comments
- Document complex algorithms with explanations
- Include examples for non-obvious patterns
- Reference related files and functions
- Explain "why" not just "what"

### Commit Messages
- Use conventional commit format
- Reference issue/PR numbers
- Include impact on success rates for generator changes
- List affected files in body

### Algorithm Documentation
- Include pseudocode or detailed steps
- List advantages and disadvantages
- Provide expected success rates
- Reference academic papers if applicable

## Current Metrics (Phase 1)

| Difficulty | Success Rate | Generation Time | Algorithm |
|-----------|-------------|----------------|-----------|
| Easy      | 40-60%      | 1-5 sec        | Backwards-shuffle |
| Medium    | 70-85%      | 5-10 sec       | Iterative Deepening |
| Hard      | <5%         | 30-60+ sec     | Backwards-shuffle (Phase 2 needed) |

## Specialized Copilot Agents

This project has 5 specialized Copilot agents configured in `.github/agents/` (following 2026 best practices):

- **@puzzle-generator**: Use for generation algorithm work
- **@testing-validation**: Use for testing and metrics
- **@solver-integration**: Use for solver optimization
- **@phase2-implementation**: Use for Phase 2 constraint satisfaction
- **@gameplay-ui**: Use for game UI and interactions

**Example Usage:**
```
@puzzle-generator Help me optimize the iterative deepening algorithm
@testing-validation Run tests and analyze success rates for Medium difficulty
@solver-integration The solver is slow for Hard puzzles, can you optimize it?
@phase2-implementation Design the constraint satisfaction system for Phase 2
@gameplay-ui Add smooth animations when vehicles move
```

See `.github/AGENT_USAGE_GUIDE.md` for complete documentation.

## Quick Commands

The project has 32 pre-configured commands in `.github/copilot-commands.json`:

- `/generate-medium` - Generate a medium puzzle
- `/test-all-difficulties` - Run comprehensive tests
- `/debug-generation-failure` - Debug generation issues
- `/phase2-status` - Check Phase 2 progress
- `/implement-game-feature` - Add gameplay features
- `/add-game-animation` - Add visual effects

See `.github/AGENT_USAGE_GUIDE.md` for complete command reference.

## Getting Help

1. **For generation problems**: Use `@puzzle-generator`
2. **For testing/metrics**: Use `@testing-validation`
3. **For solver issues**: Use `@solver-integration`
4. **For Phase 2 work**: Use `@phase2-implementation`
5. **For gameplay/UI**: Use `@gameplay-ui`

Or reference:
- `PHASE1_IMPLEMENTATION.md` for current implementation details
- `ALTERNATIVE_ALGORITHMS.md` for algorithm designs
- `GENERATOR_GUIDE.md` for usage and best practices
- `.github/AGENT_USAGE_GUIDE.md` for agent and command reference
