# Versus AI Coding

Comparing different AI coding methodologies on the same real-world task to evaluate their strengths, weaknesses, and practical effectiveness.

## Background

This project takes a real-world codebase — the raw source of [Claude Code](https://github.com/anthropics/claude-code) — stripped of build configuration, dependency declarations, and with intentionally incomplete functionality modules. The goal is to have different AI coding methodologies attempt to **make this project buildable and runnable** by completing the missing pieces.

This provides a controlled, realistic benchmark for evaluating how different AI coding approaches perform on complex, multi-module TypeScript projects.

## The Task

**Starting State**: ~1900 TypeScript/TSX source files across 30+ modules (CLI, TUI rendering, MCP server, agent SDK, tool system, etc.) with no `package.json`, no `tsconfig.json`, no build scripts, and incomplete functionality.

**Goal**: Restore the project to a buildable, launchable state, including:

- Dependency analysis and `package.json` reconstruction
- TypeScript configuration
- Build pipeline setup
- Completing missing/incomplete modules
- Fixing type errors and cross-module integration issues

## Methodologies Under Comparison

| Methodology | Description | Key Characteristics |
|---|---|---|
| [Superpowers](https://github.com/nicekid1/superpowers) | Skill-driven workflow with structured plan/execute/verify phases | Rigid process skills, brainstorming-first, TDD enforcement |
| [Get Shit Done (GSD)](https://github.com/nicekid1/gstack) | Phase-based project management with research → plan → execute → verify pipeline | Roadmap-driven, phase decomposition, integrated QA |
| [Everything Claude Code (ECC)](https://github.com/nicekid1/Everything-Claude-Code) | Comprehensive skill library covering patterns, TDD, security, and domain knowledge | Domain-specific skills, language-specific best practices, security-first |
| [Oh My Codex](https://github.com/nicekid1/oh-my-codex) | OpenAI Codex-oriented methodology with custom instructions | Codex-specific optimizations, task decomposition |

## Evaluation Dimensions

### Quantitative Metrics

| Dimension | Measurement |
|---|---|
| **Completion** | Does the project build and run successfully? |
| **Time** | Total wall-clock time to achieve a working build |
| **Token Usage** | Total tokens consumed (input + output) |
| **Iterations** | Number of user interactions / corrections needed |
| **Error Recovery** | How many build errors encountered vs. resolved |
| **Test Coverage** | Tests generated and passing |

### Qualitative Assessment

| Dimension | Criteria |
|---|---|
| **Code Quality** | Readability, consistency, adherence to patterns in existing code |
| **Architectural Integrity** | Does the solution respect the original module boundaries and design? |
| **Process Discipline** | Did the methodology follow its own prescribed workflow? |
| **Autonomy** | How much human intervention was required? |
| **Reproducibility** | Would the same approach yield similar results on a second run? |
| **Debugging Effectiveness** | How well did it diagnose and fix build/type errors? |

## Repository Structure

```
src/
├── ink/              # TUI rendering framework (Ink-based)
├── cli/              # CLI argument parsing and entry
├── entrypoints/      # CLI, MCP, SDK entry points
├── components/       # React/Ink UI components
├── screens/          # Terminal UI screens
├── tools/            # Agent tool implementations
├── assistant/        # AI assistant logic
├── context/          # Context management
├── coordinator/      # Task coordination
├── plugins/          # Plugin system
├── skills/           # Skill system
├── commands/         # Slash commands
├── query/            # Query processing
├── hooks/            # React hooks
├── state/            # State management
├── keybindings/      # Key binding configuration
├── vim/              # Vim mode
├── server/           # Server-side components
├── remote/           # Remote session support
├── bridge/           # IPC bridge
├── buddy/            # AI buddy system
├── voice/            # Voice input
├── services/         # External services
├── schemas/          # JSON schemas
├── migrations/       # Data migrations
├── native-ts/        # Native TypeScript bindings
├── memdir/           # Memory directory management
├── constants/        # Shared constants
├── outputStyles/     # Output styling
├── types/            # TypeScript type definitions
├── utils/            # Utility functions
├── bootstrap/        # Bootstrap logic
├── tasks/            # Task management
├── upstreamproxy/    # Upstream proxy
└── moreright/        # Extended utilities
```

## Running an Experiment

Each methodology should be tested under controlled conditions:

1. **Environment**: Same OS, same Claude Code / Codex CLI version, same model
2. **Starting Point**: Clean checkout of the `main` branch
3. **Prompt**: Identical initial prompt describing the task
4. **Constraints**:
   - No internet access for looking up the original Claude Code repo
   - No manual code changes by the human operator (unless the methodology requests it)
   - Single session (no restarting with context from previous attempts)
5. **Recording**: Full session log, including all tool calls and outputs

## Results Format

Each methodology + model combination is maintained on a **separate branch**, with the branch name following the format `{methodology}_{model}`:

| Branch | Methodology | Model |
|---|---|---|
| `main` | — | Base code (no modifications) |
| `superpowers_glm5.1` | Superpowers | GLM-5.1 |
| `gsd_glm5.1` | GSD | GLM-5.1 |
| `ecc_glm5.1` | ECC | GLM-5.1 |
| `oh_my_codex_glm5.1` | Oh My Codex | GLM-5.1 |
| `superpowers_sonnet-4.6` | Superpowers | Claude Sonnet 4.6 |
| `gsd_sonnet-4.6` | GSD | Claude Sonnet 4.6 |
| ... | ... | ... |

### Branch Naming Convention

```
{methodology}_{model}

# Examples
superpowers_glm5.1
oh_my_codex_gpt-4.1
ecc_o3
gsd_sonnet-4.6
```

Methodology names use lowercase with underscores. Model names match the official model identifier.

### What Each Branch Contains

Each result branch includes all code changes made during the experiment, plus documentation at the branch root:

```
{branch root}/
├── RESULT.md             # Experiment summary and assessment
├── METRICS.json          # Quantitative metrics
├── SESSION.md            # Full session narrative (if recorded)
├── src/                  # Modified source code
├── package.json          # Reconstructed (if created)
└── tsconfig.json         # Reconstructed (if created)
```

### Comparing Results

To diff a result branch against the base:

```bash
git diff main..superpowers_glm5.1 --stat
git diff main..oh_my_codex_glm5.1 --stat
```

To compare two methodologies on the same model:

```bash
git diff superpowers_glm5.1..gsd_glm5.1 --stat
```

## Contributing

1. Pick a methodology + model combination from the test matrix
2. Create a branch named `{methodology}_{model}` from `main`
3. Run the experiment under controlled conditions
4. Commit all code changes and result documentation to the branch
5. Push the branch and open a PR against `main` with the `experiment` label

## License

This project is for research and educational purposes. The source code in `src/` originates from [Claude Code](https://github.com/anthropics/claude-code) by Anthropic.
