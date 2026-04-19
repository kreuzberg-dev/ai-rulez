______________________________________________________________________

## priority: high

- Prefer subagents for non-trivial work — implementation, research, file exploration
- Use haiku for straightforward tasks (edits, searches, mechanical refactors), sonnet for judgment (architecture, debugging, review)
- Parallelize aggressively — launch independent subagents in a single message
- Work in iterations: implementation → review → adjustment; never ship after a single pass
- Write tests first when adding functionality — define expected behavior before implementing
- Write a failing test before fixing a bug — prove the bug exists, then fix it
- Run tests after every change — never assume code works without verification
- Integration tests for API surfaces, unit tests for business logic, property tests for edge-case-heavy code
