# Agent-Driven E2E Testing Reference (Ryan Carson / Yifan Zhou)

Source: https://beyondthehype.dev/p/self-healing-agents-through-e2e-testing
Gist: https://gist.github.com/yifanzz/1e00cab40fb3857764ad584343ee08c7
Tool: https://github.com/yifanzz/claude-code-boost

## The Problem
- 78% of implementations required manual debugging sessions
- Average debugging session: 3 back-and-forth rounds
- Only 11% of features got tests written automatically

## The Solution: Agent-Driven E2E Testing
Create autonomous feedback loops so the agent debugs itself.

### Results After Setup
- 11% → 95% prompts now produce tests consistently
- 29% → 71% tests acceptable on first run
- 34% → 79% tests debugged and fixed by agent itself

## Why E2E Over Unit Tests
- **Unit tests:** Agents write them ~30% of the time, miss integration issues
- **Integration tests:** Better coverage but agents struggle with realistic mocking
- **E2E tests:** Test full user journey, failure artifacts (screenshots, traces, DOM snapshots) give agents rich debugging context

## Playwright Config
```javascript
import { defineConfig } from '@playwright/test';

export default defineConfig({
  use: {
    screenshot: 'only-on-failure',
    trace: 'retain-on-failure',
  },
  webServer: {
    reuseExistingServer: false,
  },
});
```

## Testing Philosophy
- Test user-visible behavior, not implementation details
- Ensure test isolation — each test independent
- Avoid testing third-party dependencies — mock external services
- Focus on critical user journeys

## Best Practices
- **Semantic locators:** getByRole, getByText, getByLabel (NOT CSS selectors or XPath)
- **Chain and filter locators** for specificity
- **Web-first assertions:** Always await (they auto-wait and retry)
- **Descriptive test names** describing expected behavior
- **One behavior per test**
- **Arrange-act-assert** pattern
- **No hard waits** — use Playwright's built-in waiting

## Self-Debugging Loop
1. Agent writes code + tests
2. Run tests: `npx playwright test`
3. On failure: agent reads error-context.md + screenshot from test-results/
4. Agent fixes the code based on failure artifacts
5. Re-run tests
6. Loop until green (max 3 iterations)

## CLAUDE.md Snippet for Projects
Add test review guidelines + writing rules + debugging instructions to each project's AGENTS.md or CLAUDE.md. This forces the coding agent to follow E2E discipline.

## How We Apply This
- Nightly builds: Claude Code writes code + E2E tests in each session
- On failure: error output + screenshots fed back for self-debugging
- Mission board tracks which projects have test coverage
- Every new feature must ship with at least one E2E test
