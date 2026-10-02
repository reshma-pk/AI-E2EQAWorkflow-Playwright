**Agentic AI QA – End-to-End Workflow (Playwright + MCP)**

An AI-driven QA workflow that goes from a user story to committed, self-healing Playwright tests using natural-language prompts, Playwright Test Agents and MCP servers in VS Code.

Application under test: Sauce Demo (Swag Labs) – e-commerce checkout flow  
Browser: Chrome (Chromium) only

**Results at a glance**
Metric-	                                        Result
Test cases planned-	                        10 (TC-01 – TC-10)
Manual (exploratory) executed-	                10 – 9 passed, 1 failed
Automated executed-	                        10 – 6 passed, 4 expected failures (known defects)
Healing needed-	                                None – no selector or timing issues
Defects logged-	                                3 (BUG-01 – BUG-03) + 1 observation (OBS-01)

Full details: reports/ecommerce-checkout-test-report.md

**Workflow**
Step	What happens	                                Tool / Agent	                Output
1       Read and summarise the user story	        Copilot Agent	                Summary in chat
2	Create the test plan	                        playwright-test-planner	        specs/saucedemo-checkout-test-plan.md
3	Exploratory (manual) testing in a live browser	Playwright MCP	                test-evidence/exploratory-testing-results.md + screenshots
4	Generate automation scripts	                playwright-test-generator	tests/saucedemo-checkout/*.spec.js
5	Execute, classify and heal failing tests	playwright-test-healer	        test-evidence/automation-healing-results.md
6	Create the test execution report	        Copilot Agent	                reports/ecommerce-checkout-test-report.md
7	Commit and push all artifacts	                GitHub MCP	                This repository

All prompts are in QA_E2E_Prompt.md. Each step was run one at a time for reliability.

**Project structure**
.
├── .github/                        # Playwright Test Agents (planner, generator, healer)
├── .vscode/mcp.json                # MCP server config (Playwright + GitHub)
├── user_stories/
│   └── Saucedemo-ecommerce.md      # SCRUM-101 user story
├── specs/
│   └── saucedemo-checkout-test-plan.md
├── test-evidence/
│   ├── exploratory-testing-results.md
│   ├── automation-healing-results.md
│   └── screenshots/
├── tests/
│   ├── saucedemo-checkout/         # TC-01 – TC-10 automated tests
│   └── seed.spec.ts                # Seed test used by the agents (logs in)
├── reports/
│   └── ecommerce-checkout-test-report.md
├── QA_E2E_Prompt.md                # Master prompt for the full workflow
└── playwright.config.ts            # Playwright config (chromium project)

**Test cases**
ID	Test	Acceptance criteria
TC-01	Cart review	AC1
TC-02	Valid checkout	AC2, AC3
TC-03	Empty-field validation	AC2
TC-04	Invalid checkout data	AC5
TC-05	Order overview	AC3
TC-06	Cancel controls	AC3
TC-07	Browser back navigation	Technical notes
TC-08	Order completion & cart cleared	AC4, Business rule 2
TC-09	Authentication context	Business rule 1
TC-10	Boundary / multi-item totals	AC3

**Known defects**

These tests are marked with test.fail() so the suite passes while the defects stay tracked. When a defect is fixed, Playwright will flag the test so the marker can be removed.

ID	Test	Issue
BUG-01	TC-01	Cart page does not show a total price (AC1)
BUG-02	TC-04	Invalid data (special characters) is accepted at checkout (AC5)
BUG-03	TC-10	Item total shows a floating-point error, e.g. $57.980000000000004
OBS-01	TC-09	Checkout overview can be opened directly with an empty cart (not covered by an AC)

**Setup**
bash
git clone https://github.com/reshma-pk/AI-E2EQAWorkflow-Playwright.git
cd AI-E2EQAWorkflow-Playwright
npm install
npx playwright install chromium
Open the folder in VS Code.
Open .vscode/mcp.json and start the playwright-test and github servers.
When prompted, enter your GitHub PAT. It is stored securely by VS Code and is not saved in the file.

**Running the AI workflow**

Open Copilot Chat in Agent mode, attach QA_E2E_Prompt.md, and run one step at a time:

Read the prompt file QA_E2E_Prompt.md and perform only STEP 1: Read User Story, exactly as defined in that file. Do not start STEP 2 or any other step.

Repeat for STEP 2 – STEP 7.

**Running the tests**
bash
npx playwright test tests/saucedemo-checkout --project=chromium   # run the suite
npx playwright test --ui                                          # interactive UI mode
npx playwright show-report                                        # open HTML report

**Test data**
Sauce Demo public test account: standard_user / secret_sauce

**Tech stack**
Playwright · JavaScript / TypeScript · Playwright MCP · Playwright Test Agents · GitHub MCP Server · VS Code Copilot Agent mode
