# Cursor AI Pair Programmer: Setup and Workflow Guide

## Introduction

Welcome! Cursor is an AI-powered code editor designed to accelerate development through intelligent assistance, code generation, and analysis.

This document outlines a specific, effective workflow for using Cursor, based on experience gained as both a single developer and within larger organizational contexts. While primarily focused on a structured approach suitable for complex projects or solo work, the principles and configurations discussed can be adapted for team environments.

**Note:** This guide presents a *recommended* workflow. Cursor is flexible; feel free to adapt these suggestions to best suit your project needs and personal preferences.

The goal is to provide a comprehensive setup and usage pattern for developers new to Cursor, enabling you to leverage its capabilities efficiently.

## Installation and Basic Setup

### Prerequisites
- macOS
- Homebrew package manager

### Installation Steps
1. Install Cursor using Homebrew:
```bash
brew install --cask cursor
```
This allows for controlled updates rather than auto-updates.

2. Enable Language-Specific Linters
- Open Cursor Settings
- Navigate to the Language-specific settings
- Enable basic linters for your primary development languages
  
Note: These are minimal linter configurations. More aggressive linting should be handled through:
- Git pre-commit hooks
- Separate `make lint` targets

## Understanding Agent Modes

Cursor offers different operational modes to balance performance, cost, and capabilities. Understanding these is key before diving into specific workflows.

### Agent Modes
1. Standard Mode
   - Default configuration
   - Balanced performance and cost
   - Suitable for routine development tasks

2. YOLO Mode
   - Maximum performance and context
   - Uses all available resources
   - Significantly higher cost
   - Best for complex problem-solving
   - Warning: Can quickly escalate costs

3. Sequential Thinking Mode (Enabled via MCP Plugin - see Configuration)
   - Enhanced problem-solving capabilities
   - Better for complex design and implementation
   - Structured thought process
   - Recommended for:
     - Design documentation
     - Implementation planning
     - Complex problem-solving
     - Architecture decisions

### Mode Selection Guidelines
- Use Standard Mode for:
  - Routine coding
  - Simple edits
  - Quick fixes
- Use YOLO Mode for:
  - Complex debugging
  - Large refactoring
  - System-wide changes
- Use Sequential Thinking for:
  - Design documentation
  - Implementation planning
  - Complex problem analysis

## Recommended Workflow Structure

This structured approach helps maintain focus, manage complexity, and ensure thorough documentation, especially for larger features or projects. It integrates specific tools and practices for optimal results.

### Development Stages
Your development process is broken into three distinct stages, each requiring a separate Cursor session:

1. Design Documentation Stage (Session 1)
   - Create comprehensive design documents
   - Use Sequential Thinking MCP tool for design analysis
   - Document architectural decisions and system components

2. Implementation Documentation Stage (Session 2)
   - Create detailed implementation plan
   - Use Sequential Thinking MCP tool for implementation planning
   - Structure document with clear section numbering

3. Development Stage (Session 3+)
   - Implement from the implementation document
   - Use Sequential Thinking MCP tool when stuck
   - Track progress in TODO.md

### Documentation Standards
- All documentation must be in Markdown format
- Documentation should be easily readable and editable
- Design docs and implementation docs are separate concerns
- See [example-design.md](./example-design.md) for a simple design document example
- See [example-TODO.md](./example-TODO.md) for a comprehensive implementation plan example

### Conversation Formatting
- Use double backticks `` to indicate commands or console output
- This helps Cursor distinguish between:
  - Your directives and questions
  - Command examples
  - Console output
- Example:
  ```
  To run the tests, use:
  `go test ./...`
  
  The output should look like:
  `PASS
  ok      github.com/your/repo    0.123s`
  ```
- This formatting improves conversation clarity and helps Cursor better understand your intent

### Implementation Document Structure
- Use hierarchical section numbering (1, 1.1, 1.1.1, etc.)
- Each section should be self-contained and implementable in sequence
- Include pre and post-implementation directives:
  ```markdown
  Section 1: Feature X
  Pre-implementation:
  - Run linter
  - Execute unit tests
  - Verify dependencies

  Implementation steps...

  Post-implementation:
  - Run linter
  - Execute unit tests
  - Verify feature functionality
  ```

### Progress Tracking
- Maintain TODO.md for progress tracking
- Update completion status after each section
- Track any blockers or dependencies

### Implementation Workflow Strategies
- **Directing Implementation Focus**
  - Be specific when implementing from TODO.md: "Implement from section 5"
  - Direct Cursor to work on exact sections to maintain focus

- **Managing Completed Sections**
  - Mark sections as completed as you progress
  - Ask Cursor to condense or remove completed sections
  - Keep the working set small to maintain context and performance

- **Refining Implementation Details**
  - During second-pass planning, request: "Add implementation details or subtasks to each section"
  - This makes implementation easier by breaking work into smaller chunks
  - Manually review all generated documents to prevent feature creep
  - Remove unwanted sections or provide explicit reasoning: "Don't implement section X because..."

- **Prioritization Techniques**
  - Ask Cursor to assign priorities to all tasks
  - Use consistent priority notation: either "low/medium/high" or "P1/P2/P3"
  - Work with Cursor to reorder priorities based on your goals
  - Be vigilant about scope management in each implementation session

### Session Management
- Start new sessions for each major stage to maintain clear context, prevent agent confusion, and manage token usage/costs effectively.
- Recommended session breakpoints:
  1. Design document creation
  2. Implementation plan creation
  3. Development work (create new sessions as needed)
- Signs to start a new session:
  - Agent shows signs of confusion
  - Responses become inconsistent
  - Context seems lost
  - Long-running sessions (several hours)

### LLM Integration Strategy

The chosen LLMs for each stage leverage their specific strengths (e.g., ideation, coding, problem-solving) to provide comprehensive support.

#### Design Stage
Sequential use of three LLMs for comprehensive design analysis:
1. Gemini Pro 2.5
2. Claude-Sonnet-3.7 (Thinking)
3. DeepSeek R1

#### Implementation Stage
Same sequence of LLMs for implementation planning:
1. Gemini Pro 2.5
2. Claude-Sonnet-3.7 (Thinking)

#### Development Stage
Focused use of two primary LLMs:
1. Claude-Sonnet-3.7 (Thinking) - Primary editor
2. Gemini Pro 2.5 - Problem-solving assistant

#### LLM Usage Notes
- DeepSeek R1: Excellent for ideation and gap analysis, not recommended for direct programming
- Claude-Sonnet-3.7: Superior for direct code editing and implementation
- Gemini Pro 2.5: Strong support for problem-solving
- Multiple LLM perspective provides comprehensive analysis and fills potential gaps

### Sequential Thinking Mode
- Enable the Sequential Thinking MCP plugin:
  ```bash
  npx -y @modelcontextprotocol/server-sequential-thinking
  ```
- Configure in MCP settings (settings.json):
  ```json
  {
    "mcpServers": {
      "sq": {
        "command": "npx",
        "args": [
          "-y",
          "@modelcontextprotocol/server-sequential-thinking"
        ]
      }
    }
  }
  ```

#### Dependencies Installation
- Install Node.js and npm (if not already installed):
  ```bash
  brew install node
  ```
- Install required dependencies:
  ```bash
  brew install jq # for JSON processing
  npm install -g npm@latest # ensure latest npm version
  ```
- To update the sequential thinking tool to the latest version:
  ```bash
  npm cache clean --force
  npm update -g @modelcontextprotocol/server-sequential-thinking
  ```

#### Additional Resources
- Project Documentation: [Sequential Thinking MCP](https://glama.ai/mcp/servers/@arben-adm/mcp-sequential-thinking)
- The tool runs with the command: `npx -y @modelcontextprotocol/server-sequential-thinking`

#### Usage Instructions
- Activate by saying "use sequential thinking" before your query
- Recommended for:
  - Design documentation (Stage 1)
  - Implementation planning (Stage 2)
  - Complex development challenges
- Not recommended for:
  - Simple queries
  - Routine coding tasks
  - Quick fixes

#### How Sequential Thinking Works
1. Problem Definition
   - Clear articulation of the problem
   - Initial scope and constraints

2. Decomposition
   - Breaking down into sub-tasks
   - Identifying dependencies

3. Sequential Steps
   - Logical organization of tasks
   - Dependency mapping

4. Reasoning and Validation
   - Step-by-step reasoning
   - Assumption validation
   - Edge case identification

5. Iteration and Refinement
   - Process revision
   - Solution branching
   - Insight incorporation

#### Benefits
- Improved problem-solving through structured approach
- Enhanced reasoning and documentation
- Reduced errors through validation
- Increased flexibility in solution exploration
- Better maintainability and collaboration

#### Cost and Performance Considerations
- Token usage: 5x to 8x increase
- Response time: Longer due to detailed analysis
- Best combined with Claude Sonnet 3.7 (Thinking) mode
  - Small additional delay
  - Doubles token count
  - Recommended for complex tasks

## Additional Configuration
- System prompt configuration (See: [System Prompt Configuration Guide] - link to be added)

## Best Practices
- Maintain clear separation between design, implementation, and development phases
- Leverage each LLM's strengths appropriately
- Use version control for all documentation
- Regular review and updates of configuration as needed
- Create new sessions for each major development stage
- Keep implementation sections focused and testable
- Follow pre/post implementation checklists rigorously

## Usage and Pricing Considerations

### Paid Company Plans
- Enable usage-based pricing for enhanced capabilities:
  - Deeper model responses
  - Faster processing
  - Extended context windows

### Cost Management
- Usage-based pricing can escalate quickly:
  - Deep thinking modes can cost hundreds of dollars in a few hours
  - Maximum context modes significantly increase costs
  - Large context windows increase token usage and costs

### Cost Optimization Tips
- Use standard modes for routine development
- Reserve deep thinking/max modes for complex problems
- Clear context when switching tasks
- Start new sessions regularly to avoid context bloat
- Monitor usage patterns and costs

## Agent Configuration

### Required Settings
1. MCP Tools Configuration
   - Uncheck "MCP tools protection" in settings
   - This allows the agent to run MCP tools automatically
   - Required for Sequential Thinking mode to function

2. Agent Mode Settings
   - Set default mode to "Agent Mode"
   - Enable auto-run mode
   - Disable direct file deletion by default

3. Linting Configuration
   - Enable "Iterate on Lints" setting
   - Allows chat in agent mode to automatically fix linter errors
   - Improves development workflow efficiency

4. System Rules Configuration
   - Enable in "Cursor Settings -> Rules"
   - Add [RULES.md](./RULES.md) to override system rules
   - This document contains comprehensive programming guidelines and interaction rules
