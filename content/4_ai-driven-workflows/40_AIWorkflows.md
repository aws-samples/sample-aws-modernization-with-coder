---
title: "AI-Driven Game Development"
chapter: false
weight: 50
---

## Autonomous AI Development with Memory Card Game

Experience the power of Agentic AI development by working with a Memory Card Game application. In this workflow, you'll use Coder's agent-ready workspaces to autonomously develop, modify, and test a live web application using natural language prompts and Claude Code.

This scenario demonstrates how AI agents can independently analyze issues, implement solutions, run tests, and prepare code for review — all within a pre-configured development environment that starts in under a minute.

### Workflow: AI-Driven Game Modifications with Claude Code

#### Scenario: Modify a Memory Card Game using autonomous AI agents

You'll use Coder's task-based workspaces to make real changes to a running Memory Card Game, watching the AI agent work through implementation, testing, and verification autonomously.

#### Step 1: Launch Your Memorycard Workspace

1. **Access your Coder dashboard** and click "Tasks"
2. **Select the AWS Workshop Kubernetes Memorycard with Claude Code template** from the dropdown
3. **In the task prompt box**, type:

```
Start workspace and list issues in the repository at https://github.com/coder-contrib/memory-card-ai-demo/issues by issue number. Analyze each issue thoroughly and await further prompts.
```

4. **Click the submit arrow** to launch the task

Below the prompt window, you will see a task initializing. Hover over the task and click to open the workspace.

::alert[Wait for automatic provisioning (~2 minutes). The workspace will clone the repository, install dependencies, start the Vite dev server, and load issues into the AI agent's context.]{type="info"}

#### Step 2: Verify Your Environment

Once provisioned, you'll have access to:
- **AI Agent tab** — Claude Code running in a tmux session
- **Preview App tab** — The live Memory Card Game
- **VS Code Web** — Browser-based editor
- **Terminal** — Direct workspace access

**Quick verification:**
1. Select the **Preview App** tab to view the Memory Card Game
2. Click cards to flip them and match pairs
3. Open the **AI Agent** tab to confirm issues are loaded

The AI assistant should display the available issues from the repository, including:
- Issue #1014: Change card back design
- Issue #1015: Add difficulty levels (4x4, 6x6, 8x8 grids)
- Issue #1016: Implement high score persistence
- Issue #1017: Create theme selector
- Issue #1018: Add a countdown timer

#### Step 3: Make Your First Change — Card Back Design (Issue #1014)

Let's start with a simple visual change to see the agent in action. In the AI Agent tab, provide this prompt:

```
I want to work on Issue #1014 to change the card back design. Please:
1. Review the card rendering in the source code
2. Create branch: coder-contrib/issue-1014-card-back-diamond
3. Modify the card back display to show a red diamond emoji or SVG
4. Ensure it only shows when cards are face down
5. Verify the change in the running app
```

**What to observe:**
- The agent reads the codebase to understand the card component structure
- It creates a feature branch without touching main
- It makes targeted CSS/JSX changes
- The Preview App updates in real-time via Vite hot-reload
- The agent uses Playwright to verify the change rendered correctly

::alert[The AI agent operates within defined boundaries — it has read/write access to project files and can execute Node.js tooling, but follows a step-by-step workflow and will not push to main/master branches.]{type="info"}

#### Step 4: Add Game Functionality — High Score Tracking (Issue #1016)

Now let's add persistence logic. Prompt the agent:

```
I want to work on Issue #1016 to implement high score persistence. Please:
1. Add state tracking for best score (lowest number of moves)
2. Use localStorage to persist the high score between sessions
3. Display "Best: X moves" in the game UI
4. Update the high score only when the current score beats the previous best
5. Add a running total of games played
6. Test by playing through a game and verifying the score persists after reset
```

**What to observe:**
- The agent adds React state management for score tracking
- It integrates browser localStorage for persistence
- UI updates appear immediately in the Preview App
- The agent tests by simulating gameplay to verify persistence works

#### Step 5: Add Visual Polish — Theme Selector (Issue #1017)

For a more substantial change that demonstrates the agent handling multiple files:

```
I want to work on Issue #1017 to create a theme selector. Please:
1. Add theme options: Animals, Emoji, and Flags
2. Create symbol arrays for each theme:
   - Animals: 🐶🐱🐭🐹🐰🦊🐻🐼
   - Flags: 🇺🇸🇬🇧🇫🇷🇩🇪🇯🇵🇧🇷🇨🇦🇦🇺
3. Add a UI selector that appears before the game starts
4. Update the card rendering to use the selected theme
5. Verify each theme works correctly
```

**What to observe:**
- The agent modifies multiple components (UI, game logic, card rendering)
- It handles array data structures for different symbol sets
- Theme selection UI integrates naturally with existing game flow
- All three themes are tested via the Preview App

#### Step 6: Review Changes and Prepare for PR

After completing your modifications, prompt the agent to prepare for review:

```
Please prepare my work for review:
1. Stage all changes
2. Create a descriptive commit message summarizing the modifications
3. Show me a diff summary of what was changed
4. Do NOT push or create a pull request — I will handle that manually
```

::alert[The AI agent will stage changes and show you what would be committed, but all git push and PR operations are left to you. This ensures human oversight of what gets merged.]{type="warning"}

### What This Demonstrates

| Capability | How It's Shown |
|---|---|
| **Autonomous execution** | Agent independently analyzes, implements, and tests changes |
| **Live feedback loop** | Vite hot-reload shows changes instantly in Preview App |
| **Safe boundaries** | Agent works on feature branches, never pushes to main |
| **Multi-file reasoning** | Theme selector requires coordinated changes across components |
| **Verification** | Agent uses Playwright MCP to confirm changes render correctly |
| **Zero setup time** | Full dev environment ready in ~2 minutes with no manual configuration |

::alert[🚀 **Agentic Development**: These agent-ready workspaces eliminate the gap between "describe what you want" and "see it running." The combination of pre-configured environments, live preview, and autonomous AI execution means developers can iterate on ideas in minutes rather than hours.]{type="info"}

## Next Steps

Now that you've experienced autonomous AI-driven game development, continue to the next workflow to explore AI-assisted application development with Kiro CLI.
