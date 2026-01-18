# Claude Code CLI - Complete iOS Development Setup Guide

> A comprehensive guide for setting up Claude Code CLI with PRD-driven workflows, extended thinking (ultrathink), planning modes, community agent skills, essential MCP servers, and Xcode optimizations for professional Swift/SwiftUI iOS development.

---

## Table of Contents

1. [Installation](#1-installation)
2. [Configuration Hierarchy](#2-configuration-hierarchy)
3. [Essential MCP Servers](#3-essential-mcp-servers)
4. [Community Agent Skills](#4-community-agent-skills)
5. [CLAUDE.md Setup for iOS Projects](#5-claudemd-setup-for-ios-projects)
6. [PRD-Driven Development Workflow](#6-prd-driven-development-workflow)
7. [Extended Thinking & Ultrathink](#7-extended-thinking-ultrathink)
8. [Plan Mode Configuration](#8-plan-mode-configuration)
9. [Custom Slash Commands for iOS](#9-custom-slash-commands-for-ios)
10. [Subagents Configuration](#10-subagents-configuration)
11. [Output Styles](#11-output-styles)
12. [Plugins System](#12-plugins-system)
13. [Xcode Optimizations](#13-xcode-optimizations)
14. [Sandbox Mode & Safe Development](#14-sandbox-mode-safe-development)
15. [Settings & Permissions](#15-settings-permissions)
16. [Hooks for Swift Development](#16-hooks-for-swift-development)
17. [Complete Project Structure](#17-complete-project-structure)
18. [Best Practices & Tips](#18-best-practices-tips)

---

## 1. Installation

### Homebrew Installation (Recommended for macOS)

```bash
# Install via Homebrew
brew install claude

# Verify installation
claude --version
```

### Native Installer (Alternative)

```bash
# macOS/Linux - Native installer (no Node.js required)
curl -fsSL https://claude.ai/install.sh | bash

# Or install latest version
curl -fsSL https://claude.ai/install.sh | bash -s latest
```

### NPM Installation (Alternative)

```bash
# Global npm install (do NOT use sudo)
npm install -g @anthropic-ai/claude-code

# Migrate existing npm install to native
claude install
```

### Authentication

```bash
# Start Claude Code and authenticate via OAuth
claude

# Or set API key environment variable
export ANTHROPIC_API_KEY="your-key-here"
```

### Model Selection

```bash
# Use specific model at startup
claude --model claude-opus-4-5-20250929
claude --model claude-sonnet-4-5-20250929
claude --model claude-3-5-haiku-20241022

# Or set default model
export ANTHROPIC_MODEL="claude-sonnet-4-5-20250929"
```

---

## 2. Configuration Hierarchy

Claude Code uses a layered configuration system where each level can override the one below:

```
Priority (Highest to Lowest):
├── 1. Session flags (--model, --permission-mode)
├── 2. Environment variables
├── 3. .claude/settings.local.json (local - personal, gitignored)
├── 4. .claude/settings.json (project - shared with team)
└── 5. ~/.claude/settings.json (user - global)
```

### Configuration Scopes

| Scope | Description | Storage |
|-------|-------------|---------|
| **local** | Available only to you in current project (default for MCP) | `.claude/settings.local.json` |
| **project** | Shared with team via git | `.claude/settings.json`, `.mcp.json` |
| **user** | Available across all your projects | `~/.claude/settings.json` |

### Key Configuration Files

| File | Scope | Git Status | Purpose |
|------|-------|------------|---------|
| `~/.claude/settings.json` | User | N/A | Global preferences |
| `~/.claude/CLAUDE.md` | User | N/A | Global instructions |
| `~/.claude/skills/` | User | N/A | Personal Agent Skills |
| `~/.claude/commands/` | User | N/A | Personal slash commands |
| `.claude/settings.json` | Project | Committed | Team settings |
| `.claude/settings.local.json` | Local | Gitignored | Personal overrides |
| `.mcp.json` | Project | Committed | Shared MCP servers |
| `CLAUDE.md` | Project | Committed | Main project context |
| `.claude/skills/` | Project | Committed | Project Agent Skills |
| `.claude/commands/` | Project | Committed | Project slash commands |
| `.claude/agents/` | Project | Committed | Project subagents |

---

## 3. Essential MCP Servers

### Quick Setup - All Essential Servers

```bash
#!/bin/bash
# Complete MCP setup for iOS development

echo "🚀 Installing Essential MCP Servers..."

# Core iOS Development - Choose one:
# Option 1: XcodeBuildMCP (Recommended - Full featured)
npx -y @smithery/cli@latest install cameroncooke/xcodebuildmcp --client claude-code

# Option 2: xc-mcp (Alternative - Lightweight)
# claude mcp add xc-mcp -- npx -y xc-mcp

# Version Control
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Memory & Context
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory

# History
claude mcp add claude-historian-mcp -- npx claude-historian-mcp

# Verify
claude mcp list

echo "✅ Essential MCP servers installed!"
echo ""
echo "Optional servers for advanced use cases:"
echo "  - Filesystem: Advanced file operations"
echo "  - Sequential Thinking: Complex reasoning"
```

### Optional MCP Servers

These are useful for specific scenarios but not required for iOS development:

```bash
# Filesystem - Advanced file operations (useful for complex refactoring)
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem

# Sequential Thinking - Complex multi-step reasoning
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking
```

### Xcode MCP Servers

There are two main MCP servers for Xcode integration. Choose based on your needs:

#### XcodeBuildMCP (Recommended for Full Build Pipeline)

**Source:** [cameroncooke/xcodebuildmcp](https://github.com/cameroncooke/XcodeBuildMCP)

**Installation:**
```bash
# Via Smithery (Recommended)
npx -y @smithery/cli@latest install cameroncooke/xcodebuildmcp --client claude-code

# Or manual
claude mcp add xcodebuild -- npx cameroncooke/xcodebuildmcp
```

**Best for:**
- Complete build and test automation
- CI/CD integration
- Simulator management
- Runtime log capture
- Swift Package operations

**Key Tools:**
| Tool | Description |
|------|-------------|
| `mcp__xcodebuildmcp__build_sim_name_proj` | Build for simulator |
| `mcp__xcodebuildmcp__test_sim_name_proj` | Run tests |
| `mcp__xcodebuildmcp__capture_logs` | Debug runtime issues |
| `mcp__xcodebuildmcp__list_simulators` | Show available devices |
| `mcp__xcodebuildmcp__boot_simulator` | Boot a simulator |
| `mcp__xcodebuildmcp__install_app` | Install app on device |
| `mcp__xcodebuildmcp__launch_app` | Launch installed app |
| `mcp__xcodebuildmcp__screenshot` | Capture simulator screenshot |
| `mcp__xcodebuildmcp__swift_package_build` | Build Swift package |
| `mcp__xcodebuildmcp__swift_package_test` | Run Swift package tests |
| `mcp__xcodebuildmcp__clean` | Clean build products |

#### xc-mcp (Alternative: Lightweight Xcode Integration)

**Source:** [conorluddy/xc-mcp](https://github.com/conorluddy/xc-mcp)

**Installation:**
```bash
# Via npm
claude mcp add xc-mcp -- npx -y xc-mcp

# Or add to .mcp.json
```

**Best for:**
- Lightweight Xcode project interaction
- Quick builds without full pipeline
- Simpler setup for smaller projects
- Alternative to XcodeBuildMCP

**When to use which:**
- **Use XcodeBuildMCP** for production projects with full CI/CD needs
- **Use xc-mcp** for lighter-weight projects or as an alternative
- **Can use both** but typically choose one to avoid conflicts

> **Recommendation:** Start with **XcodeBuildMCP** for comprehensive iOS development. Try **xc-mcp** if you need a lighter alternative or want to experiment with different workflows.

### GitHub MCP

**Installation:**
```bash
claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

**Capabilities:**
- Create and manage issues
- Review pull requests
- Search repositories and code
- Manage branches
- Clone and fork repositories

**Usage:**
```
> Create an issue on @github:repo://owner/repo for the authentication bug
> Review @github:pr://owner/repo/123 and suggest improvements
> Search @github:code://owner/repo for "SwiftUI navigation"
```

**Authentication:**
```bash
# In Claude session
/mcp
# Select GitHub → Authenticate → Complete OAuth flow
```

### Memory MCP

**Installation:**
```bash
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory
```

**Capabilities:**
- Persistent context across sessions
- Remember project-specific decisions
- Store architectural choices
- Maintain conversation history

**Usage:**
```
> Remember that we use MVVM with @Observable for this project
> What architecture pattern did we decide on?
> Store this: We always use Swift 6 strict concurrency
```

### Claude Historian MCP

**Installation:**
```bash
claude mcp add claude-historian-mcp -- npx claude-historian-mcp
```

**Capabilities:**
- Track full conversation history
- Reference past decisions and implementations
- Search through previous sessions
- Maintain project timeline

**Usage:**
```
> What did we discuss about the networking layer last week?
> Show me the history of authentication implementation decisions
```

### Filesystem MCP (Optional)

**Installation:**
```bash
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem
```

**Capabilities:**
- Advanced file operations
- Directory watching
- Glob pattern matching
- Batch file operations

**Use Cases for iOS:**
- Complex project-wide refactoring
- Batch file renaming/reorganization
- Advanced search patterns across codebase

### Sequential Thinking MCP (Optional)

**Installation:**
```bash
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking
```

**Capabilities:**
- Multi-step reasoning
- Complex problem decomposition
- Chain-of-thought processing

**Use Cases for iOS:**
- Complex architectural decisions
- Multi-step migration planning
- Algorithm design and optimization

### Project-Level MCP Configuration

Create `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "XcodeBuildMCP": {
      "command": "npx",
      "args": ["-y", "xcodebuildmcp@latest"],
      "env": {
        "INCREMENTAL_BUILDS_ENABLED": "true",
        "XCODEBUILDMCP_SENTRY_DISABLED": "true",
        "XCODEBUILDMCP_DYNAMIC_TOOLS": "true",
        "XCODEBUILDMCP_ENABLED_WORKFLOWS": "simulator,device,project-discovery,swift-package"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "claude-historian": {
      "command": "npx",
      "args": ["claude-historian-mcp"]
    }
  }
}
```

**Alternative Configuration (using xc-mcp):**
```json
{
  "mcpServers": {
    "xc-mcp": {
      "command": "npx",
      "args": ["-y", "xc-mcp"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "claude-historian": {
      "command": "npx",
      "args": ["claude-historian-mcp"]
    }
  }
}
```

> **Tip:** Only include the MCP servers you actually use. Starting with just one Xcode server (XcodeBuildMCP or xc-mcp), GitHub, and Memory is recommended.

### Managing MCP Servers

```bash
# List all configured servers
claude mcp list

# Get details for a specific server
claude mcp get XcodeBuildMCP

# Remove a server
claude mcp remove XcodeBuildMCP

# Check server status (in session)
/mcp

# Authenticate with OAuth-enabled servers
/mcp  # Then select "Authenticate"
```

---

## 4. Community Agent Skills

Agent Skills are automatically invoked by Claude based on context. Install community skills to enhance Claude's capabilities.

### Quick Installation

```bash
#!/bin/bash
# Install all community agent skills

SKILLS_DIR="$HOME/.claude/skills"
mkdir -p "$SKILLS_DIR"
cd "$SKILLS_DIR"

# Swift Concurrency Skill
git clone https://github.com/AvdLee/Swift-Concurrency-Agent-Skill.git swift-concurrency

# Dimillian's Skills Collection
git clone https://github.com/Dimillian/Skills.git dimillian-skills

# Mobile Observability
git clone https://github.com/nexus-labs-automation/mobile-observability.git mobile-observability

# OpenSkills - CLI-based skill management compatible with Claude Code
git clone https://github.com/numman-ali/openskills.git openskills

# n-skills - Curated collection of production-ready agent skills
git clone https://github.com/numman-ali/n-skills.git n-skills

echo "✅ All skills installed!"
echo ""
echo "Additional CLI tools for skill management:"
echo "  • OpenSkills CLI: Available at ~/.claude/skills/openskills"
echo "  • n-skills: Curated collection at ~/.claude/skills/n-skills"
echo "  • Agents CLI: npm install -g @wshobson/agents"
```

### Swift Concurrency Agent Skill

**Source:** [AvdLee/Swift-Concurrency-Agent-Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)

**Installation:**
```bash
cd ~/.claude/skills/
git clone https://github.com/AvdLee/Swift-Concurrency-Agent-Skill.git swift-concurrency
```

**Capabilities:**
- Swift 6 concurrency best practices
- Actor isolation and Sendable compliance
- Async/await pattern implementation
- MainActor usage guidelines
- Data race detection and fixes
- Task group management
- AsyncSequence and AsyncStream

**Automatic Activation:**
Claude automatically uses this skill when:
- Writing concurrent code
- Fixing concurrency warnings
- Implementing async APIs
- Reviewing code for data races

**Example Usage:**
```
> Implement a concurrent image downloader that downloads multiple images safely
> Fix these Swift 6 concurrency warnings in my ViewModel
> Review this actor implementation for thread safety issues
```

### Dimillian's Skills Collection

**Source:** [Dimillian/Skills](https://github.com/Dimillian/Skills)

**Installation:**
```bash
cd ~/.claude/skills/
git clone https://github.com/Dimillian/Skills.git dimillian-skills
```

**Included Skills:**
- SwiftUI Component Library
- Navigation patterns
- Data modeling best practices
- Testing strategies
- Performance optimization
- Accessibility implementation

**Automatic Activation:**
- Building SwiftUI interfaces
- Creating reusable components
- Implementing navigation
- Writing tests

### Mobile Observability Skill

**Source:** [nexus-labs-automation/mobile-observability](https://github.com/nexus-labs-automation/mobile-observability)

**Installation:**
```bash
cd ~/.claude/skills/
git clone https://github.com/nexus-labs-automation/mobile-observability.git mobile-observability
```

**Capabilities:**
- Analytics integration (Firebase, Mixpanel, Amplitude)
- Crash reporting setup (Crashlytics, Sentry)
- Performance monitoring
- User behavior tracking
- A/B testing implementation
- Feature flagging
- Remote configuration

**Example Usage:**
```
> Set up Firebase Analytics for this screen
> Implement crash reporting with proper context
> Add performance monitoring to track app launch time
```

### n-skills - Curated Production Skills

**Source:** [numman-ali/n-skills](https://github.com/numman-ali/n-skills)

**Installation:**
```bash
cd ~/.claude/skills/
git clone https://github.com/numman-ali/n-skills.git n-skills
```

**What it is:**
n-skills is a curated collection of production-ready agent skills that have been tested and refined for real-world use. Unlike general skill repositories, n-skills focuses on battle-tested patterns and workflows that have proven effective in professional development environments.

**Key Features:**
- **Production-Ready**: All skills are tested in real-world scenarios
- **Well-Documented**: Each skill includes detailed usage examples
- **iOS-Focused**: Many skills specifically target iOS/Swift development
- **Active Maintenance**: Regularly updated with new patterns and fixes

**Included Skill Categories:**
- **Code Quality**: Linting, formatting, and code review workflows
- **Testing Patterns**: Unit test generation, UI testing strategies
- **Architecture**: MVVM, Clean Architecture, and modular design
- **Performance**: Optimization techniques and profiling
- **DevOps**: CI/CD integration, deployment automation

**Example Skills:**
- SwiftUI component generation with tests
- API client generation from OpenAPI specs
- Database migration strategies
- Localization workflow automation

**Usage:**
Skills from n-skills activate automatically based on context, or you can reference them directly:
```
> Use the n-skills API client pattern to create a networking layer
> Apply the n-skills testing strategy to this ViewModel
```

### OpenSkills CLI

**Source:** [numman-ali/openskills](https://github.com/numman-ali/openskills)

**Installation:**
```bash
cd ~/.claude/skills/
git clone https://github.com/numman-ali/openskills.git openskills
```

**What it is:**
OpenSkills is a CLI-based skill management system that closely matches Claude Code's skills format. It uses the same prompt structure, marketplace approach, and folder organization, but operates via command-line interface rather than being built into tools.

**Capabilities:**
- Skill discovery and installation via CLI
- Compatible skill format with Claude Code
- Community-driven skill marketplace
- Easy skill sharing and distribution

### Agents CLI

**Source:** [wshobson/agents](https://github.com/wshobson/agents)

**Installation:**
```bash
# Install the CLI tool
npm install -g @wshobson/agents

# Or use npx
npx @wshobson/agents
```

**What it is:**
A CLI tool for managing Claude Code agent skills, similar to OpenSkills. Provides commands for discovering, installing, and managing agent skills from various sources.

**Capabilities:**
- Command-line interface for skill management
- Browse and install skills from repositories
- Manage local skill collections
- Compatible with Claude Code skills format

### Agent Skills Registry

Browse and discover agent skills from multiple sources:

**Primary Registries:**
- **[agent-skills.md](https://agent-skills.md/)** - Community-driven skill marketplace
- **[Ai-Agent-Skills](https://github.com/skillcreatorai/Ai-Agent-Skills)** - Curated collection of AI agent skills with categorized skills for various domains

Search by technology (Swift, SwiftUI, iOS) and install with one command.

### Claude Code Templates

**Source:** [davila7/claude-code-templates](https://github.com/davila7/claude-code-templates)  
**Website:** [aitmpl.com/agents](https://www.aitmpl.com/agents)

Ready-to-use project templates and configuration files for Claude Code, including:

**What it provides:**
- Pre-configured `.claude/` directory structures
- Sample CLAUDE.md templates for various project types
- Ready-to-use `.mcp.json` configurations
- Common slash command collections
- Sample hooks and automation scripts
- Browse templates visually at [aitmpl.com/agents](https://www.aitmpl.com/agents)

**How to use:**
```bash
# Browse templates on the website first
# Visit: https://www.aitmpl.com/agents

# Clone the templates repository
git clone https://github.com/davila7/claude-code-templates.git

# Copy relevant templates to your project
cp -r claude-code-templates/ios-app/.claude ./
cp claude-code-templates/ios-app/CLAUDE.md ./

# Customize for your specific needs
```

**Benefits:**
- ✅ Jumpstart new projects with proven configurations
- ✅ Learn best practices from working examples
- ✅ Standardize setup across team projects
- ✅ Reduce initial configuration time
- ✅ Visual browsing of available templates

**Common Templates Available:**
- iOS/Swift projects
- Web applications
- Backend services
- Full-stack applications
- Data science projects

> **Tip:** Browse [aitmpl.com/agents](https://www.aitmpl.com/agents) to preview templates before downloading. Look for iOS-specific configurations that complement the setup described in this guide.

### Creating Custom Skills

Create `.claude/skills/my-custom-skill/SKILL.md`:

```markdown
---
name: my-custom-skill
description: Brief description with keywords like "SwiftUI", "networking", "Core Data" for automatic activation
allowed-tools: Read, Write, Edit, mcp__xcodebuildmcp__*
---

# My Custom Skill

## Purpose
[What this skill helps with]

## Instructions
1. [Step-by-step instructions]
2. [Claude should follow]

## Best Practices
- [Key practice 1]
- [Key practice 2]

## Code Templates
```swift
// Include reusable templates
```
```

### Managing Skills

```bash
# List installed skills
ls ~/.claude/skills/

# Update all skills
cd ~/.claude/skills/
for dir in */; do cd "$dir"; git pull; cd ..; done

# Remove a skill
rm -rf ~/.claude/skills/skill-name
```

---

## 5. CLAUDE.md Setup for iOS Projects

The `CLAUDE.md` file is your primary context provider. Claude automatically loads it at session start.

### Root CLAUDE.md Template for iOS

Create `CLAUDE.md` in your project root:

```markdown
# Project: [Your App Name]

## Quick Reference
- **Platform**: iOS 17+ / macOS 14+
- **Language**: Swift 6.0
- **UI Framework**: SwiftUI
- **Architecture**: MVVM with @Observable
- **Minimum Deployment**: iOS 17.0
- **Package Manager**: Swift Package Manager

## MCP Integration
**IMPORTANT**: This project uses the following MCP servers:
- **XcodeBuildMCP**: All Xcode build/test/simulator operations
- **GitHub**: Repository and issue management (@github:repo://owner/repo)
- **Memory**: Persistent context across sessions
- **Historian**: Conversation history and past decisions

## Agent Skills Active
- **Swift Concurrency**: async/await, actors, Sendable compliance
- **Dimillian's SwiftUI Components**: Reusable UI patterns
- **Mobile Observability**: Analytics, crash reporting, monitoring
- **n-skills**: Production-ready patterns and workflows
- **OpenSkills**: Best practices and community standards

## Build Commands
- Build: `mcp__xcodebuildmcp__build_sim_name_proj`
- Test: `mcp__xcodebuildmcp__test_sim_name_proj`
- Clean: `mcp__xcodebuildmcp__clean`
- Logs: `mcp__xcodebuildmcp__capture_logs`

## Project Structure
\`\`\`
MyApp/
├── App/                    # App entry point
├── Features/               # Feature modules (MVVM)
│   ├── [FeatureName]/
│   │   ├── Views/          # SwiftUI views
│   │   ├── ViewModels/     # @Observable classes
│   │   └── Models/         # Data models
├── Core/                   # Shared utilities
│   ├── Extensions/
│   ├── Services/
│   └── Networking/
├── Resources/              # Assets, Localizations
└── Tests/
\`\`\`

## Coding Standards

### Swift 6 Concurrency
- Use strict concurrency checking
- Mark types as Sendable where appropriate
- Use @MainActor for UI-bound code
- Prefer actors over DispatchQueue
- Use async/await over completion handlers
- Never use `DispatchQueue.main.async { @MainActor in ... }`

### SwiftUI Patterns
- Extract views when they exceed 100 lines
- Use @State for local view state only
- Use @Environment for dependency injection
- Prefer NavigationStack over deprecated NavigationView
- Use @Bindable for bindings to @Observable objects
- Always provide accessibility labels

### Navigation Pattern
```swift
// Use NavigationStack with type-safe routing
enum Route: Hashable {
    case detail(Item)
    case settings
}

NavigationStack(path: $router.path) {
    ContentView()
        .navigationDestination(for: Route.self) { route in
            // Handle routing
        }
}
```

### Error Handling
```swift
// Always use typed errors
enum AppError: LocalizedError {
    case networkError(underlying: Error)
    case validationError(message: String)
    
    var errorDescription: String? {
        switch self {
        case .networkError(let error): return error.localizedDescription
        case .validationError(let msg): return msg
        }
    }
}
```

## Testing Requirements
- Unit tests for all ViewModels (80%+ coverage)
- UI tests for critical user flows
- Use Swift Testing framework (@Test, #expect)
- Mock network and external dependencies
- Test concurrency with Task { }

## DO NOT
- Use deprecated APIs (UIKit when SwiftUI suffices)
- Create massive monolithic views
- Use force unwrapping (!) without documentation
- Ignore Swift 6 concurrency warnings
- Write completion-based async code
- Use ObservableObject (use @Observable instead)
- Use Combine (use async/await instead)

## Planning Workflow
When starting new features:
1. Read the PRD from `docs/PRD.md`
2. Create feature spec in `docs/specs/[feature-name].md`
3. Use `ultrathink` for architectural decisions
4. Use Plan Mode (`Shift+Tab`) for implementation strategy
5. Implement incrementally with tests
6. Use Memory MCP to store important decisions

## Memory Imports
```
@import docs/PRD.md
@import docs/ARCHITECTURE.md
@import docs/ROADMAP.md
```

---

## 6. PRD-Driven Development Workflow

### Directory Structure for PRD Workflow

```
docs/
├── PRD.md                      # Main Product Requirements Document
├── ARCHITECTURE.md             # System architecture decisions
├── ROADMAP.md                  # Development roadmap & priorities
├── specs/                      # Feature specifications
│   ├── 000-project-setup.md
│   ├── 001-authentication.md
│   ├── 002-dashboard.md
│   └── template.md
└── tasks/                      # Task breakdowns
    ├── 000-sample.md
    └── [feature]-tasks.md
```

### PRD Template (`docs/PRD.md`)

```markdown
# Product Requirements Document: [App Name]

## Executive Summary
[Brief description of the product and its primary value proposition]

## Problem Statement
[What problem does this solve? Who experiences this problem?]

## Target Users
- **Primary**: [Description]
- **Secondary**: [Description]

## Success Metrics
| Metric | Target | Measurement |
|--------|--------|-------------|
| User Retention | 40% D7 | Analytics |
| App Rating | 4.5+ | App Store |
| Crash-Free Rate | 99.5% | Crashlytics |

## Core Features

### Feature 1: [Name]
**Priority**: P0 (Must Have)
**Description**: [Detailed description]
**User Stories**:
- As a [user type], I want [action] so that [benefit]

**Acceptance Criteria**:
- [ ] Criterion 1
- [ ] Criterion 2

**Technical Requirements**:
- iOS 17+ required
- Offline support needed
- Data persistence via SwiftData

## Non-Functional Requirements
- **Performance**: App launch < 2s, smooth 60fps scrolling
- **Accessibility**: WCAG 2.1 AA compliance
- **Localization**: English (primary), [other languages]
- **Security**: Keychain for credentials, certificate pinning

## Out of Scope (v1.0)
- [Feature explicitly not included]

## Technical Constraints
- Swift 6.0+ with strict concurrency
- SwiftUI-only (no UIKit unless necessary)
- SwiftData for persistence
- Minimum iOS 17.0

## Timeline
| Phase | Duration | Deliverables |
|-------|----------|--------------|
| Design | 2 weeks | Figma mockups |
| Development | 8 weeks | MVP features |
| Testing | 2 weeks | QA sign-off |
| Launch | 1 week | App Store submission |
```

### PRD-Driven Slash Commands

Create `.claude/commands/` with these files:

**`.claude/commands/create-prd.md`**
```markdown
---
description: Create a new PRD from requirements discussion
allowed-tools: Read, Write, Edit
---

# Create Product Requirements Document

Based on our discussion, create a comprehensive PRD in `docs/PRD.md`.

Follow this structure:
1. Executive Summary
2. Problem Statement
3. Target Users
4. Success Metrics
5. Core Features with user stories and acceptance criteria
6. Non-Functional Requirements
7. Technical Constraints
8. Timeline

Ask clarifying questions before writing. Use ultrathink for comprehensive planning.
```

**`.claude/commands/generate-spec.md`**
```markdown
---
description: Generate feature specification from PRD
argument-hint: <feature-name>
allowed-tools: Read, Write
---

# Generate Feature Specification

Read the PRD at `docs/PRD.md` and create a detailed specification for: $ARGUMENTS

1. Extract relevant user stories and requirements
2. Define acceptance criteria
3. Design technical architecture
4. Create the spec file at `docs/specs/$ARGUMENTS.md`

ultrathink about the technical design before writing.
```

---

## 7. Extended Thinking & Ultrathink

Extended thinking is **disabled by default** in Claude Code. You can enable it on-demand.

### Enabling Extended Thinking

| Method | Description |
|--------|-------------|
| **Tab key** | Press `Tab` to toggle Thinking on/off during session |
| **Prompt triggers** | Use phrases like "think", "think hard", "ultrathink" |
| **Environment variable** | Set `MAX_THINKING_TOKENS` for permanent enablement |

### Thinking Budget Hierarchy

| Keyword | Budget | Best For |
|---------|--------|----------|
| `think` | ~4K tokens | Simple planning, quick decisions |
| `think hard` / `megathink` | ~10K tokens | Medium complexity |
| `think harder` / `think longer` | ~16K tokens | Complex analysis |
| `ultrathink` | ~32K tokens (max) | Architecture, critical debugging |

### Usage Examples

```
# Simple planning
"Think about how to structure this view"

# Medium complexity  
"Think hard about the navigation architecture for this feature"

# Complex architectural decisions
"Ultrathink about how to design the offline sync system for this app"
```

### When to Use Ultrathink

✅ **Use Ultrathink for:**
- System architecture decisions
- Complex debugging (e.g., race conditions, memory leaks)
- Large-scale refactoring planning
- Migration strategies (UIKit → SwiftUI, CoreData → SwiftData)
- Performance optimization analysis

❌ **Avoid Ultrathink for:**
- Simple code changes
- Well-specified tasks with clear steps
- Rapid prototyping iterations

---

## 8. Plan Mode Configuration

### What is Plan Mode?

Plan Mode instructs Claude to analyze codebases with **read-only operations** - perfect for:
- Multi-step implementation planning
- Code exploration before making changes
- Interactive development with iterative direction refinement

### Enabling Plan Mode

```bash
# Start new session in Plan Mode
claude --permission-mode plan

# Or use -p flag for headless Plan Mode query
claude --permission-mode plan -p "Analyze the authentication system and suggest improvements"
```

### During Session

- Press `Shift+Tab` to cycle: Normal → Auto-Accept → Plan Mode
- Plan Mode indicator: `⏸ plan mode on`
- Auto-Accept indicator: `⏵⏵ accept edits on`

### Opus Plan Mode Strategy

Use Opus for planning, Sonnet for execution:

```bash
# Plan with Opus
claude --model claude-opus-4-5-20250929 --permission-mode plan

# Execute with Sonnet (more cost-effective)
claude --model claude-sonnet-4-5-20250929
```

---

## 9. Custom Slash Commands for iOS

### Project Commands (`.claude/commands/`)

**build.md**
```markdown
---
description: Build the iOS project intelligently
allowed-tools: mcp__xcodebuildmcp__*
---

# Build Project

Detect the project type and build appropriately:
1. Check for .xcworkspace or .xcodeproj
2. Use XcodeBuildMCP to build for iOS Simulator
3. Report any build errors with suggested fixes
```

**test.md**
```markdown
---
description: Run all relevant tests
allowed-tools: mcp__xcodebuildmcp__*, Bash(swift *)
---

# Run Tests

Based on current context:
- For main app: `mcp__xcodebuildmcp__test_sim_name_proj`
- For Swift packages: `mcp__xcodebuildmcp__swift_package_test`
- Report test results and any failures
```

**run-app.md**
```markdown
---
description: Build and launch app on simulator
allowed-tools: mcp__xcodebuildmcp__*
---

# Build and Run

1. List available simulators
2. Build the app for the default simulator (iPhone 15)
3. Boot the simulator if needed
4. Install and launch the app
5. Start capturing logs
```

**create-view.md**
```markdown
---
description: Create a new SwiftUI view with ViewModel
argument-hint: <ViewName>
allowed-tools: Read, Write
---

# Create SwiftUI View: $ARGUMENTS

Create a new SwiftUI view following project patterns:

1. Read existing views for style reference
2. Create `$ARGUMENTS.swift` in appropriate Features directory
3. Create `${ARGUMENTS}ViewModel.swift` as @Observable class
4. Add preview provider
5. Follow project's navigation and styling patterns
```

---

## 10. Subagents Configuration

Subagents are specialized AI assistants invoked to handle specific task types.

### Creating iOS-Specific Subagents

**`.claude/agents/ios-architect.md`**
```markdown
---
name: ios-architect
description: iOS architecture expert for system design and patterns
model: claude-opus-4-5-20250929
tools: Read, Grep, Glob
---

You are an expert iOS architect specializing in Swift and SwiftUI.

Your expertise includes:
- MVVM, MVC, VIPER, Clean Architecture
- Swift Concurrency (actors, async/await, Sendable)
- SwiftUI navigation patterns
- Dependency injection
- SwiftData and Core Data
- Modular app architecture

When consulted:
1. Analyze the existing codebase structure
2. Consider scalability and maintainability
3. Propose patterns that fit the team's expertise
4. Provide concrete Swift code examples
5. Consider testing implications
```

---

## 11. Output Styles

Output Styles adapt Claude Code for different use cases.

### Built-in Output Styles

| Style | Description |
|-------|-------------|
| **Default** | Standard software engineering focus |
| **Explanatory** | Provides educational "Insights" while coding |
| **Learning** | Collaborative learn-by-doing with `TODO(human)` markers |

### Changing Output Style

```bash
# Open style menu
/output-style

# Or switch directly
/output-style explanatory
/output-style learning
```

---

## 12. Plugins System

Plugins extend Claude Code with custom functionality.

### Installing Plugins

```bash
# Open plugin manager
/plugin

# Add custom marketplace
claude plugin marketplace add owner/repo

# Install directly
claude plugin install plugin-name@marketplace-name
```

---

## 13. Xcode Optimizations

Based on [Oleksandr Khorbushko's Guide](https://khorbushko.github.io/article/2021/02/01/make-xCode-great-again.html) with updates for Swift 6 and Xcode 15+

### Performance Optimizations

Add these to your project's build settings or `.xcode.env` file:

**Build Performance**
```bash
# Increase concurrent compile tasks (adjust based on your Mac's cores)
export IDEBuildOperationMaxNumberOfConcurrentCompileTasks=8

# Enable build timing
export CLANG_ENABLE_BUILD_TIMING=YES

# Swift compilation optimization
export SWIFT_COMPILATION_MODE=wholemodule  # Release
export SWIFT_COMPILATION_MODE=incremental  # Debug

# Enable module caching
export SWIFT_MODULE_CACHING_ENABLED=YES

# Swift 6 specific optimizations
export SWIFT_ENFORCE_EXCLUSIVE_ACCESS=on  # Memory safety
export SWIFT_DETERMINISTIC_HASHING=1      # Reproducible builds

# Enable explicit modules (Xcode 15+)
export SWIFT_ENABLE_EXPLICIT_MODULES=YES

# Reduce index time (CI/CD only - disables code completion locally)
export COMPILER_INDEX_STORE_ENABLE=NO
```

**Swift 6 Concurrency Settings**
```bash
# Enable strict concurrency checking
SWIFT_STRICT_CONCURRENCY=complete

# Enable upcoming features
SWIFT_UPCOMING_FEATURE_CONCISE_MAGIC_FILE=YES
SWIFT_UPCOMING_FEATURE_EXIST_ANY=YES
SWIFT_UPCOMING_FEATURE_IMPLICIT_OPEN_EXISTENTIALS=YES
```

**Xcode 15+ Specific Optimizations**
```bash
# Enable explicit modules for faster incremental builds
SWIFT_ENABLE_EXPLICIT_MODULES=YES

# Use new build system features
BUILD_LIBRARY_FOR_DISTRIBUTION=NO  # Unless building a framework

# Enable background compilation
COMPILER_INDEX_STORE_ENABLE=YES
INDEX_ENABLE_BUILD_ARENA=YES

# Optimize asset catalog compilation
ASSETCATALOG_COMPILER_OPTIMIZATION=space  # or 'time' for faster builds
```

### EditorConfig for Xcode

**Source:** [Pol Piella's Xcode EditorConfig Guide](https://www.polpiella.dev/xcode-editor-config/)

EditorConfig provides consistent coding styles across different editors and IDEs, including Xcode (14.3+).

**Installation:**
Create `.editorconfig` in your project root:

```ini
# EditorConfig for iOS/Swift Projects
# https://editorconfig.org

root = true

# All files
[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

# Swift files
[*.swift]
indent_style = space
indent_size = 4
max_line_length = 120

# SwiftUI files (same as Swift)
[*.swift]
indent_style = space
indent_size = 4

# Xcode project files
[*.{xcodeproj,xcworkspace}/**.pbxproj]
indent_style = tab

# YAML files
[*.{yml,yaml}]
indent_style = space
indent_size = 2

# JSON files
[*.json]
indent_style = space
indent_size = 2

# Markdown files
[*.md]
trim_trailing_whitespace = false
max_line_length = off

# Plist files
[*.plist]
indent_style = tab

# Strings files
[*.strings]
indent_style = space
indent_size = 2
```

**Key Benefits:**
- **Automatic Formatting**: Xcode 14.3+ respects EditorConfig settings automatically
- **Team Consistency**: All team members get the same formatting regardless of their Xcode settings
- **Language-Specific**: Different rules for Swift, YAML, JSON, etc.
- **Version Control Friendly**: Committed to repo, applies to all contributors

**Xcode Integration:**
- Xcode 14.3+ has built-in EditorConfig support
- No plugins or extensions needed
- Settings are applied automatically when opening files
- Overrides local Xcode text editing preferences for files in the project

**Common Settings Explained:**

| Setting | Options | Description |
|---------|---------|-------------|
| `indent_style` | space, tab | Use spaces or tabs for indentation |
| `indent_size` | number | Number of spaces per indent level |
| `max_line_length` | number, off | Maximum line length (Xcode warns beyond this) |
| `end_of_line` | lf, crlf, cr | Line ending style (lf = Unix/macOS) |
| `charset` | utf-8, etc. | File character encoding |
| `trim_trailing_whitespace` | true, false | Remove whitespace at end of lines |
| `insert_final_newline` | true, false | Ensure file ends with newline |

**Advanced Swift Configuration:**
```ini
# Swift files with strict formatting
[*.swift]
indent_style = space
indent_size = 4
max_line_length = 120
trim_trailing_whitespace = true
insert_final_newline = true

# Enforce consistent line endings
end_of_line = lf

# Match Swift API Design Guidelines spacing
# (Xcode 15+ supports additional Swift-specific rules)
```

**Verification:**
```bash
# Check if EditorConfig is working
# Open a Swift file in Xcode and check:
# Xcode > Settings > Text Editing > Indentation
# It should show "(using EditorConfig)" when active
```

**Integration with Other Tools:**
EditorConfig works alongside:
- SwiftLint (formatting rules)
- swift-format (code formatting)
- Xcode's built-in formatting (Ctrl+I)

**Best Practices:**
1. Commit `.editorconfig` to version control
2. Keep settings consistent with your SwiftLint rules
3. Document any project-specific deviations
4. Review EditorConfig when onboarding new team members

### Xcode Settings (Preferences)

**General:**
- Navigation: Uses Focused Editor
- Double Click Navigation: Uses Separate Window
- Issues: Show Live Issues ✓
- Enable: Automatically trim trailing whitespace
- Enable: Including whitespace-only lines

**Text Editing:**
- Enable: Code folding ribbon
- Enable: Line numbers
- Enable: Code completion
- Enable: Show type information
- Indentation: 4 spaces (not tabs)
- Enable: Automatically balance brackets

**Behaviors (Xcode > Settings > Behaviors):**
- Build Starts: Hide navigator
- Build Succeeds: Show navigator (+ play sound for motivation!)
- Build Fails: Show issue navigator + beep
- Testing: Show test navigator
- Run Completes: Show debug navigator

**Locations:**
- DerivedData: Custom location (ideally RAM disk)
- Archives: ~/Library/Developer/Xcode/Archives (default)

**Key Bindings (Optional but Recommended):**
- ⌘ + Shift + O: Open Quickly
- ⌘ + Shift + J: Reveal in Project Navigator  
- ⌘ + Shift + F: Find in Project
- ^ + ⌘ + ↑: Switch between .swift and Test file
- ⌘ + K: Clear Console

### SwiftLint Configuration

Create `.swiftlint.yml` with Swift 6 optimizations:

```yaml
# SwiftLint Configuration for Swift 6 Projects

included:
  - Sources
  - App
  - Features
  - Core
  
excluded:
  - Carthage
  - Pods
  - DerivedData
  - .build
  - Tests/MockData
  - "**/*.generated.swift"

analyzer_rules:
  - unused_declaration
  - unused_import

disabled_rules:
  - trailing_whitespace
  - todo
  
opt_in_rules:
  - array_init
  - closure_spacing
  - empty_count
  - explicit_init
  - first_where
  - force_unwrapping
  - sorted_first_last
  - strict_fileprivate
  - unavailable_function
  - unneeded_parentheses_in_closure_argument
  - vertical_parameter_alignment_on_call
  # Swift 6 Concurrency
  - actor_isolated
  - sendable_function_not_sendable
  - missing_sendable_annotation

line_length:
  warning: 120
  error: 200
  ignores_comments: true
  ignores_urls: true

file_length:
  warning: 500
  error: 1000
  ignore_comment_only_lines: true

type_body_length:
  warning: 300
  error: 500

function_body_length:
  warning: 50
  error: 100

cyclomatic_complexity:
  warning: 10
  error: 20
  ignores_case_statements: true

identifier_name:
  min_length:
    warning: 2
  max_length:
    warning: 40
    error: 50
  excluded:
    - id
    - url
    - URL
    - db
    - x
    - y
    - i
    - j

# Swift 6 specific
strict_concurrency: warning
actor_isolated: warning

reporter: "xcode"
```

### Build Scripts

**Pre-Build: SwiftLint**
```bash
#!/bin/bash
if which swiftlint >/dev/null; then
    swiftlint
else
    echo "warning: SwiftLint not installed"
fi
```

**Pre-Build: Swift Format Check**
```bash
#!/bin/bash
if which swift-format >/dev/null; then
    swift-format lint --recursive Sources/
else
    echo "warning: swift-format not installed, run: brew install swift-format"
fi
```

### Swift-Format Configuration

Create `.swift-format` in project root:

```json
{
  "version": 1,
  "lineLength": 120,
  "indentation": {
    "spaces": 4
  },
  "respectsExistingLineBreaks": true,
  "lineBreakBeforeControlFlowKeywords": false,
  "lineBreakBeforeEachArgument": true,
  "lineBreakBeforeEachGenericRequirement": false,
  "prioritizeKeepingFunctionOutputTogether": true,
  "indentConditionalCompilationBlocks": true,
  "lineBreakAroundMultilineExpressionChainComponents": true,
  "rules": {
    "AllPublicDeclarationsHaveDocumentation": false,
    "AlwaysUseLowerCamelCase": true,
    "AmbiguousTrailingClosureOverload": true,
    "BeginDocumentationCommentWithOneLineSummary": true,
    "DoNotUseSemicolons": true,
    "DontRepeatTypeInStaticProperties": true,
    "FileScopedDeclarationPrivacy": true,
    "FullyIndirectEnum": true,
    "GroupNumericLiterals": true,
    "IdentifiersMustBeASCII": true,
    "NeverForceUnwrap": false,
    "NeverUseForceTry": false,
    "NeverUseImplicitlyUnwrappedOptionals": false,
    "NoAccessLevelOnExtensionDeclaration": true,
    "NoBlockComments": true,
    "NoCasesWithOnlyFallthrough": true,
    "NoEmptyTrailingClosureParentheses": true,
    "NoLabelsInCasePatterns": true,
    "NoLeadingUnderscores": false,
    "NoParensAroundConditions": true,
    "NoVoidReturnOnFunctionSignature": true,
    "OneCasePerLine": true,
    "OneVariableDeclarationPerLine": true,
    "OnlyOneTrailingClosureArgument": true,
    "OrderedImports": true,
    "ReturnVoidInsteadOfEmptyTuple": true,
    "UseLetInEveryBoundCaseVariable": true,
    "UseShorthandTypeNames": true,
    "UseSingleLinePropertyGetter": true,
    "UseSynthesizedInitializer": true,
    "UseTripleSlashForDocumentationComments": true,
    "UseWhereClausesInForLoops": true,
    "ValidateDocumentationComments": true
  }
}
```

### Additional Tooling

**Install Recommended Tools:**
```bash
# SwiftLint
brew install swiftlint

# Swift Format
brew install swift-format

# Periphery (finds unused code)
brew install periphery

# XcodeGen (project generation from YAML)
brew install xcodegen
```

**Periphery Configuration (`.periphery.yml`):**
```yaml
# Find unused code
targets:
  - MyApp
schemes:
  - MyApp
retain_public: false
disable_redundant_public_analysis: false
clean_build: false
```

---

## 14. Sandbox Mode & Safe Development

### Sandbox Configuration Levels

#### Level 1: Full Sandbox (Read + Build Only)

`.claude/settings.json`:
```json
{
  "model": "claude-sonnet-4-5-20250929",
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "mcp__xcodebuildmcp__build_*",
      "mcp__xcodebuildmcp__test_*",
      "mcp__xcodebuildmcp__list_*",
      "Bash(git status)",
      "Bash(git diff *)"
    ],
    "deny": [
      "Write", "Edit",
      "Bash(rm *)",
      "Bash(git push *)"
    ]
  }
}
```

#### Level 2: Sandbox + Docs

```json
{
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "Write(docs/*)",
      "Edit(docs/*)",
      "mcp__xcodebuildmcp__*"
    ],
    "deny": [
      "Write(*.swift)",
      "Edit(*.swift)"
    ]
  }
}
```

### Using Sandbox Mode

```bash
# Start in Plan Mode (read-only)
claude --permission-mode plan

# Return to normal development
claude
```

---

## 15. Settings & Permissions

### Project Settings (`.claude/settings.json`)

```json
{
  "model": "claude-sonnet-4-5-20250929",
  "permissions": {
    "allow": [
      "mcp__xcodebuildmcp__*",
      "Read", "Write", "Edit",
      "Bash(git *)",
      "Bash(swift *)",
      "Bash(swiftlint *)"
    ],
    "deny": [
      "Read(.env*)",
      "Write(.env*)",
      "Bash(rm -rf *)"
    ]
  },
  "env": {
    "PROJECT_NAME": "MyApp",
    "DEFAULT_SIMULATOR": "iPhone 15",
    "SWIFT_VERSION": "6.0",
    "IOS_DEPLOYMENT_TARGET": "17.0"
  }
}
```

---

## 16. Hooks for Swift Development

Hooks execute at various points in Claude Code's lifecycle.

### Configuration

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/session-start.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | { read f; [[ \"$f\" == *.swift ]] && swiftlint lint --path \"$f\" --quiet; }"
          }
        ]
      }
    ]
  }
}
```

### Hook Scripts

**`.claude/hooks/session-start.sh`**
```bash
#!/bin/bash

PROJECT_NAME=$(basename "$(pwd)")
SWIFT_VERSION=$(swift --version 2>/dev/null | head -1)

echo "🚀 Starting $PROJECT_NAME session" >&2
echo "📱 $SWIFT_VERSION" >&2

# Check if simulator is booted
if ! xcrun simctl list devices booted | grep -q "Booted"; then
    echo "💡 No simulator running. Use /run-app to boot one." >&2
fi
```

---

## 17. Complete Project Structure

```
MyiOSApp/
├── .claude/
│   ├── commands/                    # Project slash commands
│   │   ├── build.md
│   │   ├── test.md
│   │   ├── run-app.md
│   │   ├── create-view.md
│   │   └── create-prd.md
│   ├── agents/                      # Project subagents
│   │   ├── ios-architect.md
│   │   └── swift-reviewer.md
│   ├── skills/                      # Project Agent Skills
│   │   └── ios-testing/
│   │       └── SKILL.md
│   ├── hooks/                       # Automation hooks
│   │   └── session-start.sh
│   ├── settings.json               # Team settings (committed)
│   └── settings.local.json         # Personal settings (gitignored)
├── .mcp.json                        # MCP server configuration
├── .editorconfig                    # EditorConfig for consistent formatting
├── CLAUDE.md                        # Main project context
├── docs/
│   ├── PRD.md
│   ├── ARCHITECTURE.md
│   ├── specs/
│   └── tasks/
├── MyApp/
│   ├── App/
│   ├── Features/
│   ├── Core/
│   └── Resources/
├── MyAppTests/
├── Package.swift
└── .swiftlint.yml
```

### Recommended `.gitignore` additions

```gitignore
# Claude Code
.claude/settings.local.json
.claude/*.log

# Keep these committed
!.claude/commands/
!.claude/agents/
!.claude/hooks/
!.claude/settings.json
```

### Git Integration Best Practices

For optimal Xcode and Git workflow, see [thoughtbot's Xcode and Git guide](https://thoughtbot.com/blog/xcode-and-git-bridging-the-gap):

**Key Tips:**
- Use `.gitattributes` for proper project file merging
- Configure Xcode to use relative paths
- Avoid committing user-specific settings (`.xcuserstate`)
- Use Git LFS for large assets if needed

**Recommended `.gitattributes`:**
```gitattributes
*.pbxproj merge=union
*.xcscheme merge=union
*.xcworkspacedata merge=union
```

This helps prevent merge conflicts in Xcode project files.

---

## 18. Best Practices & Tips

> **📚 Recommended Reading:** For comprehensive iOS development best practices beyond Claude Code setup, see the [Infinum iOS Handbook](https://infinum.com/handbook/ios) which covers architecture patterns, testing strategies, and team workflows.

### Workflow Best Practices

1. **Start with Planning**
   - Use Plan Mode for new features
   - Write specs before code
   - Use `ultrathink` for architectural decisions

2. **Incremental Development**
   - Complete one task at a time
   - Test after each change
   - Commit frequently

3. **Leverage Agent Skills**
   - Swift Concurrency skill for async code
   - Mobile Observability for analytics
   - n-skills for production-ready patterns
   - Skills activate automatically

4. **Use MCP Servers Effectively**
   - XcodeBuildMCP for all Xcode operations
   - GitHub MCP for issue/PR management
   - Memory MCP for cross-session context

### Prompt Patterns

```
# Starting a new feature with concurrency
"Read the PRD and spec for authentication. Ultrathink about the implementation. 
Use Swift Concurrency skill best practices. Create a task breakdown."

# Debugging with logs
"The app crashes when logging in. Capture logs with XcodeBuildMCP and 
ultrathink to identify the root cause. Check for concurrency issues."

# Using GitHub integration
"Review the PR at @github:pr://owner/repo/123 and provide feedback on 
the authentication implementation."

# Storing decisions
"Remember: We decided to use SwiftData with CloudKit sync. Store this 
decision for future reference."
```

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Tab` | Toggle extended thinking on/off |
| `Shift+Tab` | Cycle permission modes |
| `Ctrl+C` | Cancel current operation |
| `Ctrl+O` | Toggle verbose mode (see thinking) |
| `/` | Open slash command menu |
| `@` | Reference files, directories, MCP resources |

### Common Commands

| Command | Description |
|---------|-------------|
| `/help` | Show all available commands |
| `/clear` | Clear conversation context |
| `/compact` | Summarize and compress context |
| `/cost` | Show token usage and cost |
| `/model` | Change model mid-session |
| `/mcp` | Manage MCP servers |
| `/build` | Build project (custom command) |
| `/test` | Run tests (custom command) |

### Troubleshooting

```bash
# Check Claude Code health
claude doctor

# Debug MCP connections
claude --mcp-debug

# View session logs
tail -f ~/.claude/logs/session.log

# Reset configuration
rm -rf ~/.claude && claude
```

---

## Quick Start Checklist

- [ ] Install Claude Code CLI (native method)
- [ ] Authenticate with Claude account
- [ ] Install all essential MCP servers (run setup script)
- [ ] Install community agent skills (Swift Concurrency, Dimillian, n-skills, etc.)
- [ ] Create project `.claude/` directory structure
- [ ] Create root `CLAUDE.md` with project context
- [ ] Create `.mcp.json` with MCP server configuration
- [ ] Create `.editorconfig` for consistent formatting
- [ ] Create essential slash commands (build, test, run-app)
- [ ] Set up hooks for Swift linting
- [ ] Configure SwiftLint with `.swiftlint.yml`
- [ ] Apply Xcode optimizations
- [ ] Create `docs/` structure for PRD workflow
- [ ] Write initial PRD
- [ ] Configure permissions in `settings.json`
- [ ] Test: `/build` and `/run-app`

---

## Resources

### Official Documentation
- [Claude Code Official Docs](https://code.claude.com/docs)
- [XcodeBuildMCP GitHub](https://github.com/cameroncooke/XcodeBuildMCP)
- [xc-mcp GitHub](https://github.com/conorluddy/xc-mcp)
- [MCP Documentation](https://modelcontextprotocol.io)

### Apple Developer Resources
- [Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/)
- [Swift Evolution Proposals](https://github.com/apple/swift-evolution)
- [SwiftUI Documentation](https://developer.apple.com/documentation/swiftui)
- [Swift Concurrency Guide](https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html)

### Community Skills & Tools
- [AvdLee's Swift Concurrency Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)
- [Dimillian's Skills Collection](https://github.com/Dimillian/Skills)
- [Nexus Labs Mobile Observability](https://github.com/nexus-labs-automation/mobile-observability)
- [n-skills - Production Skills](https://github.com/numman-ali/n-skills) - Curated production-ready agent skills
- [OpenSkills - CLI Tool](https://github.com/numman-ali/openskills) - CLI for managing skills
- [Agents CLI](https://github.com/wshobson/agents) - CLI tool for agent skill management
- [Claude Code Templates](https://github.com/davila7/claude-code-templates) - Ready-to-use project templates and configurations
- [AI Templates](https://www.aitmpl.com/agents) - Visual browser for Claude Code templates
- [Agent Skills Registry](https://agent-skills.md/)
- [Ai-Agent-Skills](https://github.com/skillcreatorai/Ai-Agent-Skills) - Curated AI agent skills collection

### Xcode & Development Workflow
- [Make Xcode Great Again](https://khorbushko.github.io/article/2021/02/01/make-xCode-great-again.html) - Xcode performance optimization
- [Xcode EditorConfig Setup](https://www.polpiella.dev/xcode-editor-config/) - Consistent formatting with EditorConfig
- [Xcode and Git: Bridging the Gap](https://thoughtbot.com/blog/xcode-and-git-bridging-the-gap) - Git integration best practices for Xcode projects
- [Infinum iOS Handbook](https://infinum.com/handbook/ios) - Comprehensive iOS development guide covering architecture, testing, CI/CD, and best practices

### iOS Development Best Practices
- [Kodeco Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)
- [Airbnb Swift Style Guide](https://github.com/airbnb/swift)
- [Swift.org Documentation](https://swift.org/documentation/)

---

<br>

**Inspired by and builds upon:** [Onur Keskin's Claude Code iOS Dev Guide](https://github.com/keskinonur/claude-code-ios-dev-guide)

## Acknowledgments

- [Anthropic](https://anthropic.com) for Claude Code CLI
- [Cameron Cooke](https://github.com/cameroncooke) for XcodeBuildMCP
- [Conor Luddy](https://github.com/conorluddy) for xc-mcp
- [Antoine van der Lee](https://github.com/AvdLee) for Swift Concurrency Skill
- [Thomas Ricouard (Dimillian)](https://github.com/Dimillian) for Skills Collection
- [Nexus Labs](https://github.com/nexus-labs-automation) for Mobile Observability
- [Numan Ali](https://github.com/numman-ali) for n-skills and OpenSkills
- [skillcreatorai](https://github.com/skillcreatorai) for Ai-Agent-Skills collection
- [Carlos Davila](https://github.com/davila7) for Claude Code Templates
- [Pol Piella](https://www.polpiella.dev) for EditorConfig Xcode guide
- [Oleksandr Khorbushko](https://khorbushko.github.io) for Xcode optimization guide
- [thoughtbot](https://thoughtbot.com) for Xcode/Git integration guide
- [Infinum](https://infinum.com) for comprehensive iOS handbook
- The entire iOS development community

---

*Last Updated: January 2026*  
*Claude Code Version: 2.1.7*
