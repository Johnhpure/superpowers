# Antigravity IDE Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your Antigravity equivalent:

| Skill references | Antigravity IDE equivalent |
|-----------------|---------------------------|
| `Read` (file reading) | `view_file` |
| `Write` (file creation) | `write_to_file` |
| `Edit` (file editing) | `replace_file_content` / `multi_replace_file_content` |
| `Bash` (run commands) | `run_command` |
| `Grep` (search file content) | `grep_search` |
| `Glob` (search files by name) | `find_by_name` |
| `TodoWrite` (task tracking) | Create artifact markdown with `- [ ]` checkbox syntax |
| `Skill` tool (invoke a skill) | `view_file` on the SKILL.md path |
| `WebSearch` | `search_web` or `mcp_ddg-search_search` |
| `WebFetch` | `read_url_content` or `mcp_ddg-search_fetch_content` |
| `Task` tool (dispatch subagent) | No equivalent — Antigravity does not support general-purpose subagents |

## No subagent support

Antigravity IDE has `browser_subagent` for browser-only tasks, but no equivalent to Claude Code's `Task` tool for general subagent dispatch. Skills that rely on subagent dispatch (`subagent-driven-development`, `dispatching-parallel-agents`) will fall back to single-session execution via `executing-plans`.

## Additional Antigravity tools

These tools are available in Antigravity but have no Claude Code equivalent:

| Tool | Purpose |
|------|---------|
| `list_dir` | List files and subdirectories |
| `generate_image` | Generate images for UI mockups |
| `browser_subagent` | Automate browser interactions |
| `mcp_serena_*` | Semantic code analysis (find_symbol, find_referencing_symbols, etc.) |
| `mcp_sequential-thinking_sequentialthinking` | Deep structured thinking |
| `mcp_context7_*` | Query library documentation |
| `mcp_StitchMCP_*` | UI design generation |

## TodoWrite Replacement

Since Antigravity has no `TodoWrite` tool, use artifact markdown files for task tracking:

```markdown
## Task List — [Feature Name]

- [ ] Step 1: Write failing test
- [ ] Step 2: Run test to verify it fails  
- [x] Step 3: Write minimal implementation
- [ ] Step 4: Run test to verify it passes
- [ ] Step 5: Commit
```

Store task lists as artifacts in the conversation's artifact directory.
