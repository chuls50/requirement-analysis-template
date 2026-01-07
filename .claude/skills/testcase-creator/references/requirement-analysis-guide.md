# Requirement Analysis Guide

This guide provides detailed instructions for extracting requirements from Gherkin scenarios and mapping them to test cases for the testcase-creator skill.

## Analysis Process

### Step 1: Parse Gherkin Scenarios

For each scenario in the user story, identify:

- **Given**: Preconditions (setup/context, environment state)
- **When**: Actions (user interactions, triggers, events)
- **Then**: Expected outcomes (assertions, results, state changes)
- **And**: Additional conditions or steps (can appear in any section)

**Key principle:** Multiple "And" clauses should be analyzed for whether they represent separate requirements or combined verification steps.

### Step 2: Extract Requirements

Break down each scenario into discrete, testable requirements. Each requirement should:

- Be specific and measurable
- Start with what "should" happen
- Avoid vague language ("works correctly", "functions properly")
- Capture a single testable aspect when possible

**Example scenario:**

```gherkin
Scenario: User submits login form
Given the user is on the login page
When the user enters valid credentials
And clicks the login button
Then the dashboard is displayed
And a welcome message appears
```

**Extracted requirements:**

1. Login form should accept valid credentials
2. Login button should trigger authentication process
3. System should navigate to dashboard on successful login
4. System should display welcome message after successful login

### Step 3: Identify Combination Opportunities

Analyze extracted requirements to determine if they should be:

- **Combined** into a single test case (when they represent a sequential flow)
- **Separated** into multiple test cases (when they test independent behaviors)

**Combination Strategy Guidelines:**

**✅ Combine when:**

- Requirements follow a natural user workflow
- Testing them separately would require redundant setup
- They represent verification of multiple aspects of the same interaction
- The combined test case remains clear and maintainable

**❌ Separate when:**

- Requirements test different user paths or scenarios
- They require different preconditions or environment states
- Failure of one requirement shouldn't block testing others
- They test error conditions vs. success conditions

### Step 4: Map Requirements to Test Cases

Create the mapping using hierarchical numbering: `<Scenario>.<Requirement>.<TestCase>`

**Example with combination:**

```
Scenario 1: Successful login flow
  1.1 Complete login flow should authenticate user and display dashboard
    1.1.1 Verify successful login with valid credentials
    (This test case validates all 4 requirements in a single flow)

Scenario 2: Login validation errors
  2.1 Invalid credentials should display error message
    2.1.1 Verify error message for invalid credentials
  2.2 Error message should be dismissible
    2.2.1 Verify error message dismissal
```

## Output Format

### Combined Requirements Analysis and Test Cases Table

The requirement analysis output should use a single comprehensive table that shows the full traceability:

```
| Scenario Name | Requirement | Test Case |
|---------------|-------------|-----------|
| Scenario 1: <Name> | 1.1 <First requirement description> | 1.1.1 <Test case title> |
|  | 1.2 <Second requirement> | 1.2.1 <Test case title> |
|  |  | 1.2.2 <Another test case for same requirement> |
|
| Scenario 2: <Name> | 2.1 <First requirement> | 2.1.1 <Test case title> |
```

**Formatting rules:**

- Use empty cells (`|  |`) to continue within same scenario or requirement
- Use empty row with just `|` to visually separate scenarios
- Keep requirement descriptions action-oriented and specific
- Number scenarios sequentially as they appear in user story
- Number requirements hierarchically (1.1, 1.2, 2.1, 2.2...)
- Number test cases under requirements (1.1.1, 1.1.2, 1.2.1...)

### Detailed Test Cases Table

```
| Test Case ID | Test Scenario | Expected Result | Test Data |
|--------------|---------------|-----------------|-----------|
| 1.1.1 | Enter valid email in email field | Email is accepted and validation passes | user@example.com |
| 1.1.2 | Enter invalid email in email field | Validation error message is displayed | invalid-email |
```

### Test Coverage Summary Section

After the Detailed Test Cases table, include a summary section that captures key metrics:

```
## Test Coverage Summary

| Metric | Count |
|--------|-------|
| Total Scenarios | <number> |
| Total Requirements | <number> |
| Total Test Cases | <number> |
| CSV Test Cases | <number> |

**Notes:**
- Brief explanation of the analysis approach
- Note about grouping/combination strategy used
- Any relevant context about traceability
```

**Metric Definitions:**

- **Total Scenarios**: Count of all Gherkin scenarios in the user story
- **Total Requirements**: Count of all requirement rows in the Requirements Analysis table (x.y format)
- **Total Test Cases**: Count of all test case IDs defined in the Requirements Analysis table (x.y.z format)
- **CSV Test Cases**: Count of actual test cases in the CSV file (may be less than Total Test Cases if test case IDs were combined)

**Purpose:**

This summary provides:

- Quick visibility into test coverage scope
- Metrics for reporting and tracking
- Documentation of the combination/grouping strategy
- Complete picture of scenarios → requirements → test cases mapping

**Guidelines:**

- Test Case ID matches the numbering from requirements table
- Test Scenario describes the action being tested
- Expected Result is specific and verifiable
- Test Data lists required input (use "N/A" if none needed)

## Real-World Examples

### Example 1: UI Component Verification (Combined Approach)

**User Story Scenario:**

```gherkin
Scenario 1: Default authentication method (forms based)
Given the application is configured for forms based authentication
When application loads at start up
Then the welcome screen is visible with the following components:
- username field
- password field
- sign-in button
- 'Forgot your password?' button
- Copyright text
```

**Requirement Extraction:**

- All welcome screen components should be displayed when configured for forms-based authentication

**Test Case Mapping:**

```
| Scenario Name | Requirement | Test Case |
|---------------|-------------|-----------|
| Scenario 1: Default authentication method | 1.1 Welcome screen should display all required components when configured for forms-based authentication | 1.1.1 Verify all welcome screen components display correctly on startup |
```

**Detailed Test Case:**

```
| Test Case ID | Test Scenario | Expected Result | Test Data |
|--------------|---------------|-----------------|-----------|
| 1.1.1 | Launch application configured for forms-based authentication and observe all welcome screen components | Welcome screen displays with username field, password field, Sign-In button, 'Forgot your password?' button, and copyright text | Application configured for forms-based auth |
```

**Rationale:** Since all components are part of the same screen load verification, they're combined into one test case. The test will verify all components in a single execution, making it efficient while maintaining complete coverage.

### Example 2: Error Handling (Combined Approach)

**User Story Scenario:**

```gherkin
Scenario 3: User authenticates unsuccessfully (forms based)
Given the user enters invalid credentials
When the user clicks the Sign-In button
Then the application displays an error to the user
And highlights the username and password fields red, with alert icon
And the user will have the ability to close the error message by selecting "x"
```

**Requirement Extraction:**

- Invalid credentials should display error with field highlighting and closable message

**Test Case Mapping:**

```
| Scenario Name | Requirement | Test Case |
|---------------|-------------|-----------|
| Scenario 3: User authenticates unsuccessfully | 3.1 Invalid credentials should display error with field highlighting and closable message | 3.1.1 Verify failed authentication displays error with red field highlighting and closable "x" |
```

**Detailed Test Case:**

```
| Test Case ID | Test Scenario | Expected Result | Test Data |
|--------------|---------------|-----------------|-----------|
| 3.1.1 | Enter invalid username and password, click Sign-In button, observe error display and field highlighting, then click "x" to close | Error message is displayed, username and password fields are highlighted red with alert icon, error message closes when "x" is clicked | Invalid username and password |
```

**Rationale:** All three aspects (error display, field highlighting, dismissal) are part of the same error handling flow and should be tested together as a cohesive user experience.

### Example 3: License-Based Behavior (Separated Approach)

**User Story Scenarios:**

```gherkin
Scenario 6: eNcounter License
Given an eNcounter license is activated on the host machine
When the loading process completes
Then the application displays the 'eNcounter' variant of the welcome/log in screen

Scenario 7: ClearSteth License
Given a ClearSteth license is activated on the host machine
When the loading process completes
Then the application displays the 'ClearSteth' variant of the welcome/log in screen
```

**Requirement Extraction:**

- eNcounter license should display eNcounter variant
- ClearSteth license should display ClearSteth variant

**Test Case Mapping:**

```
| Scenario Name | Requirement | Test Case |
|---------------|-------------|-----------|
| Scenario 6: eNcounter License | 6.1 Application should display eNcounter variant of welcome/log in screen with active eNcounter license | 6.1.1 Verify eNcounter variant of welcome screen displays with eNcounter license |
|
| Scenario 7: ClearSteth License | 7.1 Application should display ClearSteth variant of welcome/log in screen with active ClearSteth license | 7.1.1 Verify ClearSteth variant of welcome screen displays with ClearSteth license |
```

**Rationale:** Different licenses require different environment setup and test different code paths, so they must be separate test cases. They cannot be meaningfully combined.

### Example 4: Simple Navigation Flow (Combined Approach)

**User Story Scenario:**

```gherkin
Scenario 1: Workflow options are displayed
Given the user enters valid credentials
When the user authenticates successfully
Then the "Let's Get Started" screen is displayed with the following options:
* Patient Consultation
* Direct-to-Patient Exam
* Video Conference
```

**Requirement Extraction:**

- Application should display "Let's Get Started" screen after successful authentication with all workflow options

**Test Case Mapping:**

```
| Scenario Name | Requirement | Test Case |
|---------------|-------------|-----------|
| Scenario 1: Workflow options are displayed | 1.1 Application should display "Let's Get Started" screen after successful authentication | 1.1.1 Verify "Let's Get Started" screen is displayed after successful login |
```

**Detailed Test Case:**

```
| Test Case ID | Test Scenario | Expected Result | Test Data |
|--------------|---------------|-----------------|-----------|
| 1.1.1 | Enter valid credentials in username and password fields, click Sign-In button, observe screen after authentication | "Let's Get Started" screen is displayed with Patient Consultation, Direct-to-Patient Exam, and Video Conference options | Valid username and password |
```

**Rationale:** The authentication and screen display are part of a single sequential flow. Verifying the options are displayed is part of verifying the screen loaded correctly.

## Advanced Scenarios

### Handling Multiple "And" Clauses

When a Gherkin scenario has multiple "And" statements, analyze each one:

**Example:**

```gherkin
Then the dashboard is displayed
And the user name is shown in header
And recent items are loaded
And notifications badge shows count
```

**Analysis questions:**

1. Are these all part of verifying the dashboard loaded correctly? → **Combine**
2. Do they test independent features that could fail separately? → **Consider separating**
3. Is the test case still maintainable if combined? → **If yes, combine**

**Decision:** In this case, combine into "Verify dashboard loads with all components" because they all verify the same page load event.

### Handling Conditional Scenarios

**Example:**

```gherkin
Scenario 5: Not Licensed (Default to ClearSteth Listen-Only)
Given no license is activated on the host machine
When the loading process completes
Then the application opens as ClearSteth with listen-only functionality

Scenario 6: eNcounter License
Given an eNcounter license is activated on the host machine
When the loading process completes
Then the application displays the 'eNcounter' variant
```

**Approach:** Create separate test cases for each condition:

- Test case 1: Behavior with no license (ClearSteth listen-only)
- Test case 2: Behavior with eNcounter license
- Test case 3: Behavior with ClearSteth license

**Rationale:** Each license state requires different test environment setup and tests different system behavior.

### Handling Navigation Flows

**Example:**

```gherkin
Scenario 2: Patient Consultation
Given the user is on the Let's Get Started screen
When the user clicks the Patient Consultation option
Then the application navigates to the Select Patient screen
```

**Combine into:** "Verify Patient Consultation navigation"

- This tests both the navigation action AND validates the destination
- They can't be meaningfully separated (can't verify destination without navigating to it)
- The flow is simple enough that one test case covers it completely

## Anti-Patterns and Best Practices

### ❌ Anti-Pattern: Over-Granular Test Cases

Creating separate test cases for every minor detail:

```
Test Case 1: Verify button exists
Test Case 2: Verify button color
Test Case 3: Verify button enabled state
Test Case 4: Verify button label text
Test Case 5: Verify button click handler
```

### ✅ Best Practice: Logical Grouping

```
Test Case 1: Verify submit button properties and functionality
- Check button exists and is visible
- Verify button color matches design spec
- Verify button is enabled
- Verify button label is "Submit"
- Click button and verify form submission triggered
```

### ❌ Anti-Pattern: Under-Specified Requirements

```
Requirement: System should work correctly
Requirement: Button should function properly
Requirement: Page should load
```

### ✅ Best Practice: Specific, Testable Requirements

```
Requirement: System should display validation error when email format is invalid
Requirement: Submit button should trigger form submission and display loading indicator
Requirement: Login page should load and display all components within 3 seconds
```

### ❌ Anti-Pattern: Combining Unrelated Requirements

```
Test Case: Verify login, user profile, and logout functionality
- Enter credentials and login
- Navigate to profile page
- Update profile information
- Logout of application
```

### ✅ Best Practice: Maintain Test Case Focus

```
Test Case 1: Verify complete login flow
Test Case 2: Verify profile page operations
Test Case 3: Verify logout functionality
```

## Quality Checklist

Before finalizing requirement analysis:

**Completeness:**

- [ ] All Gherkin scenarios are represented
- [ ] Each requirement is testable and specific
- [ ] Requirements are numbered hierarchically
- [ ] Test cases combine requirements where logical
- [ ] Test case count is minimized without sacrificing coverage
- [ ] Requirement descriptions are clear and action-oriented
- [ ] Test case titles clearly indicate what is being tested
- [ ] Tables are properly formatted with empty cells and separators
