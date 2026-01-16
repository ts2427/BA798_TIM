# BA798 Repository

Welcome to the BA798 repository. This guide explains how to use Claude in VS Code to efficiently add and manage code in this repository.

## Setting Up Claude in VS Code

### Prerequisites
- VS Code installed
- Claude Code extension installed (search "Claude" in VS Code extensions)
- An active Claude API key

### Installation Steps
1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X / Cmd+Shift+X)
3. Search for "Claude" and install the official Claude extension
4. Configure your API key in the extension settings
5. You're ready to start using Claude!

## How to Use Claude with This Repository

### Opening Claude in VS Code
- Use the keyboard shortcut `Ctrl+K Cmd+K` (macOS) or `Ctrl+K Ctrl+K` (Windows/Linux)
- Or click the Claude icon in the activity bar
- You can also use the command palette: `Ctrl+Shift+P` → "Claude: Open Chat"

### Common Tasks

#### 1. **Adding a New Feature**
Describe what you want to add in the Claude chat:
```
"Add a new function that [description]. I want it in [file path]"
```
Claude will:
- Examine existing code patterns
- Write the new feature following your project's style
- Help with testing and integration

#### 2. **Fixing Bugs**
```
"There's a bug in [file path] where [description]. Please fix it."
```
Claude will:
- Analyze the issue
- Propose a fix
- Explain what was wrong

#### 3. **Understanding Code**
Ask Claude to explain existing code:
```
"Explain what the function [name] does in [file path]"
```

#### 4. **Refactoring Code**
```
"Refactor [file path] to [goal, e.g., improve readability]"
```

### Best Practices

**Before starting a task:**
- Read relevant files first by including them in context
- Be specific about what you want (file paths, desired behavior)
- Mention any constraints (performance, compatibility, etc.)

**During implementation:**
- Claude can modify multiple files at once
- Review changes before committing
- Test code locally before merging

**After Claude makes changes:**
1. Review the code carefully
2. Run tests if applicable
3. Test locally if needed
4. Use `git diff` to see exactly what changed
5. Commit with a clear message explaining the changes

### Git Workflow

#### Creating a Feature Branch
From the command line or VS Code terminal:
```bash
git checkout -b feature/your-feature-name
```

#### Making Changes with Claude
1. Open Claude in VS Code
2. Describe what you want to implement
3. Claude will write/modify code
4. Review the changes in the editor
5. Save files (Claude may do this automatically)

#### Committing Changes
```bash
git add .
git commit -m "Add feature: [description]"
```

#### Pushing and Creating a Pull Request
```bash
git push -u origin feature/your-feature-name
```
Then create a PR on GitHub/your platform with a clear description.

## Using Claude's Tools Effectively

### Slash Commands
Claude supports various slash commands for specific tasks:
- Type `/` in the chat to see available commands
- Examples: `/edit`, `/review`, `/explain`

### Multi-File Editing
Claude can modify multiple files in one request:
```
"Update [file1] and [file2] to [goal]"
```

### Code Review
Ask Claude to review your code:
```
"Review this code for bugs, performance, and style issues"
```

### Testing
Claude can help write tests:
```
"Write unit tests for the [function name] function in [file path]"
```

## Tips for Success

1. **Be Specific**: Include file paths and line numbers when referencing code
2. **Provide Context**: Share relevant portions of existing code
3. **One Task at a Time**: Complex tasks work better when broken into smaller steps
4. **Review Everything**: Always check Claude's output before committing
5. **Iterate**: If the result isn't perfect, ask Claude to refine it
6. **Use Git**: Keep track of changes with meaningful commits

## Troubleshooting

**Claude can't find a file**
- Make sure the file path is correct relative to the project root
- Check that the file exists

**Code doesn't compile/run**
- Claude may need more context about dependencies or the project structure
- Provide error messages so Claude can help debug

**Want to undo Claude's changes**
- Use `git checkout filename` to revert a file
- Or use `git reset` for uncommitted changes

## Resources

- [Claude Code Documentation](https://claude.com/claude-code)
- [VS Code Extension Guide](https://code.visualstudio.com/docs/editor/extension-marketplace)
- [Git Documentation](https://git-scm.com/doc)

---

Happy coding! Use Claude to build amazing things efficiently.
