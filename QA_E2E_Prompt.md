## End-to-End QA Workflow with Natural Language
Workflow Overview

This prompt guides you through a complete 7-step QA workflow using MCP servers and AI agents to go from user story to committed automated test scripts.

Rules for every step

Run tests in Chrome (chromium) only.
Complete one step fully and give a short status update before starting the next.
If a browser action does not respond within 60 seconds, mark that check as BLOCKED, note it and move on.

# STEP 1: Read User Story

Prompt: I need to start a new testing workflow. Please read the user story from the file: user_stories/Saucedemo-ecommerce.md

Summarize the key requirements, acceptance criteria, and testing scope.

Expected Output:

Summary of the user story
List of acceptance criteria
Application URL and test credentials
Key features to test

# STEP 2: Create Test Plan

Prompt: Based on the user story Saucedemo-ecommerce that we just reviewed, use the playwright-test-planner agent to:

Read the application URL and test credentials from the user story
Explore the application and understand all workflows mentioned in the acceptance criteria
Create a comprehensive test plan that covers all acceptance criteria including:
Happy path scenarios
Negative scenarios (validation errors, empty fields, invalid data)
Edge cases and boundary conditions
Navigation flow tests (including browser back button)
UI element validation
Save the test plan as: specs/saucedemo-checkout-test-plan.md

Ensure each test scenario includes:

Test case ID (e.g. TC-01) and clear title
The acceptance criterion it covers (AC1–AC5)
Detailed step-by-step instructions
Expected results for each step
Test data requirements

Expected Output:

Complete test plan markdown file saved to specs/
Organized test scenarios with clear structure
Browser exploration screenshots (if needed)

# STEP 3: Perform Exploratory Testing

Prompt: Now I need to perform manual exploratory testing using Playwright MCP browser tools.

Please read the test plan from: specs/saucedemo-checkout-test-plan.md

Keep the browser at a desktop size of 1280x720 for all checks. Do not resize the browser.

Then execute the test scenarios defined in that plan:

Use Playwright browser tools to manually execute each test scenario from the plan
Follow the step-by-step instructions in each test case
Verify expected results match actual results
Take screenshots at key steps and error states and save them in: test-evidence/screenshots/
Document your findings in: test-evidence/exploratory-testing-results.md
Test execution results for each scenario (PASS / FAIL / BLOCKED)
Any UI inconsistencies or unexpected behaviors
Missing validations or bugs discovered
Element selectors that worked reliably
Screenshots as evidence

Expected Output:

Manual test execution results
Screenshots of the application at various states
List of observations and findings
Any issues discovered during exploration

# STEP 4: Generate Automation Scripts

Prompt: Now I need to create automated test scripts using the playwright-test-generator agent.

Please review:

Test plan from: specs/saucedemo-checkout-test-plan.md (for test scenarios and steps)
Exploratory testing results from: test-evidence/exploratory-testing-results.md (for actual element selectors and UI insights)

Using insights from the manual exploratory testing:

Leverage the element selectors and locators that were successfully used in Step 3
Use stable element properties (IDs, data-test attributes, roles) discovered during exploration
Apply wait strategies and UI behaviors observed during manual testing
Incorporate any workarounds for UI quirks discovered

Generate Playwright JavaScript automation scripts:

Create scripts for each test scenario from the test plan
Organize scripts into appropriate test suite files in: tests/saucedemo-checkout/
Use the test case IDs, names and steps from the test plan
Use reliable selectors and strategies from exploratory testing

Requirements for all scripts:

Follow Playwright best practices
Include proper assertions using expect()
Use descriptive test names matching the format in the test plan
Use robust element selectors discovered during manual testing
Add comments for complex steps
Use proper wait strategies based on actual application behavior (no fixed waitForTimeout)
Add proper test hooks (beforeEach, afterEach)
Run on Chrome (chromium project) only

After generating the scripts, run them once to check they execute: npx playwright test tests/saucedemo-checkout --project=chromium --reporter=line

Expected Output:

Test suite files created in tests/saucedemo-checkout/ based on test plan scenarios
Scripts using robust selectors discovered during exploratory testing
All scripts follow Playwright best practices
Initial test generation complete

# STEP 5: Execute and Heal Automation Tests

Prompt: Now I need to execute the generated automation scripts and heal any failures using the playwright-test-healer agent.

Run all automation scripts: npx playwright test tests/saucedemo-checkout --project=chromium --reporter=line
Identify any failing tests
For each failing test, use the playwright-test-healer agent to:
Analyze the failure (selector issues, timing issues, assertion failures)
Auto-heal the test by fixing selectors or adding proper waits
Update the test script with the fixes
Make at most 3 healing attempts per test. If it still fails, mark it with test.fixme() and a comment explaining why, then move on
If the failure is caused by a real application bug (the app does not meet the acceptance criteria), do NOT change the assertion to make it pass. Keep the test, mark it as a defect and record it for the report
Re-run the healed tests to verify they pass
Document:
Initial test results (pass/fail count)
Healing activities performed
Final test results after healing
Any tests that couldn't be auto-healed, and any tests failing because of real defects

Expected Output:

All automation tests executed
Failing tests identified and healed using test-healer agent
Healed test scripts updated in tests/saucedemo-checkout/
Final stable test execution results
Summary of healing activities performed

# STEP 6: Create Test Report

Prompt: Now I need to create a comprehensive test execution report based on manual testing, automation execution, and healing activities.

Please compile results from:

Step 3: Manual exploratory testing results (test-evidence/exploratory-testing-results.md)
Step 4: Generated automation scripts
Step 5: Automated test execution and healing results

Save the report as: reports/ecommerce-checkout-test-report.md

Include:

Executive Summary
Total test cases planned
Test cases executed (manual + automated)
Overall Pass/Fail/Blocked status
Manual Test Results
Results from Step 3 exploratory testing
Screenshots and observations (link to files in test-evidence/screenshots/)
Issues found during manual testing
Automated Test Results
Initial automation results from Step 5
Healing activities performed
Final test execution results after healing
Test suite execution summary
Pass/Fail count for each test suite
Defects Log
For any failed tests (manual or automated):
Bug ID
Severity (Critical/High/Medium/Low)
Title and Description
Steps to Reproduce
Expected vs Actual Behavior
Screenshots/Evidence
Environment Details
Test Coverage Analysis
Which acceptance criteria are covered
Coverage from manual vs automated tests
Any gaps in test coverage
Recommendations for additional testing
Summary and Recommendations
Overall quality assessment
Risk areas
Next steps

Expected Output:

Comprehensive test execution report covering both manual and automated testing
Clear PASS/FAIL status for all test scenarios
Detailed bug reports for failures
Complete test coverage analysis
Evidence and screenshots attached

# STEP 7: Commit to Git Repository

Git Repository URL: https://github.com/reshma-pk/AI-E2EQAWorkflow-Playwright.git

Prompt: Now I need to commit all the test artifacts to the Git repository using the GitHub MCP server.

Git Repository URL: https://github.com/reshma-pk/AI-E2EQAWorkflow-Playwright.git

Please perform the following Git operations:

Initialize Git repository if not already initialized
Stage all new and modified files in the workspace, respecting .gitignore (do not commit node_modules/, test-results/, playwright-report/ or any tokens/secrets)
Create a commit with the message: "feat(tests): Add complete test suite for Saucedemo checkout workflow
Add user story documentation
Add comprehensive test plan with all scenarios
Add exploratory testing results and screenshots
Add test execution report with results
Add automated test scripts for checkout process
Include validation, navigation, and edge case tests
Resolves Saucedemo"
Push all changes to the Git repository
Provide a summary of what was committed

Expected Output:

All workspace files committed to Git
Descriptive commit message following conventional commit format
Confirmation of successful push to the provided repository
Summary of changes
Complete Workflow Execution
Single Combined Prompt (for Video Demo):

I want to demonstrate a complete end-to-end QA workflow using natural language and MCP servers. Run everything in Chrome (chromium) only.

STEP 1 - READ USER STORY: First, read the user story from: user_stories/Saucedemo-ecommerce.md Provide a brief summary of what needs to be tested.

STEP 2 - CREATE TEST PLAN: Use the playwright-test-planner agent to create a comprehensive test plan based on the user story. The agent should explore the application URL from the user story and cover all acceptance criteria. Save it as: specs/saucedemo-checkout-test-plan.md

STEP 3 - EXPLORATORY TESTING: Read the test plan from specs/saucedemo-checkout-test-plan.md and use Playwright browser tools (at 1280x720, no resizing) to manually execute each test scenario. Save screenshots to test-evidence/screenshots/ and findings to test-evidence/exploratory-testing-results.md.

STEP 4 - GENERATE AUTOMATION SCRIPTS: Review both the test plan and the exploratory testing results. Use the playwright-test-generator agent to create TypeScript automation scripts leveraging the element selectors and insights discovered during manual testing. Save scripts in tests/saucedemo-checkout/.

STEP 5 - EXECUTE AND HEAL TESTS: Run tests/saucedemo-checkout/ with --project=chromium. Use the playwright-test-healer agent to auto-heal failing tests (max 3 attempts per test, then test.fixme with a reason). Do not change assertions to hide real application defects - log them instead. Document healing activities.

STEP 6 - CREATE TEST REPORT: Create a comprehensive test execution report at: reports/ecommerce-checkout-test-report.md Compile results from Step 3 (manual testing), Step 4 (script generation), and Step 5 (execution and healing). Include PASS/FAIL status, healing summary, defects log, and test coverage analysis.

STEP 7 - COMMIT TO GIT: Use the GitHub MCP server to commit all new files (respecting .gitignore) with a descriptive message and push to https://github.com/reshma-pk/AI-E2EQAWorkflow-Playwright.git

Execute this complete workflow and provide status updates after each step.

See task progress for longer tasks.

Saucedemo-ecommerce.md
QA_E2E_Prompt.md
README.md
E2E_QA_Workflow_Prompts.docx

Track tools and referenced files used in this task.
