---
name: testcase-creator
description: Generate requirements-based test cases from Gherkin user stories using BDD methodology. Use this skill when the user asks to create test cases, generate test cases from user stories, analyze requirements for testing, convert user stories to test cases, or work with files in docs/user-stories/. This skill performs comprehensive requirement analysis and outputs Azure DevOps-compatible CSV test case files with proper formatting for import.
---

# Test Case Creator

Transform Gherkin user stories into comprehensive, requirements-based test cases through systematic requirement analysis.

## Overview

This skill implements a streamlined workflow that:

1. Analyzes Gherkin scenarios to identify testable requirements
2. Generates concise requirement analysis documentation
3. Creates Azure DevOps-compatible CSV test cases with proper formatting

## Workflow

Given a user story file with Gherkin scenarios:

1. **Requirement Analysis**: Extract and map requirements from each scenario
2. **Requirement Documentation**: Create a concise requirement analysis file
3. **Test Case Generation**: Convert requirements into detailed, executable test cases in CSV format

## Input Requirements

**Expected file structure:**

```
docs/
  └── user-stories/
      └── <ID>_<title>.us.txt
```

**User story format:**

- File must contain Gherkin scenarios (Given/When/Then syntax)
- Each scenario should have clear acceptance criteria
- File naming: `<UserStoryID>_<descriptive_title>.us.txt` (e.g., `eNr_118556_loading_screen.us.txt`)

**If Gherkin format is incomplete or missing:**

- Use the `@bdd` skill to help convert requirements to proper Gherkin syntax
- Validate Gherkin structure before proceeding
- Prompt user for clarification on ambiguous requirements

## Output Requirements

Generate two files with consistent naming based on input:

### 1. Requirement Analysis File

**Location:** `docs/requirement-analysis/`
**Naming:** `<UserStoryID>_<title>.RequirementAnalysis.txt`
**Format:** Plain text with markdown-style tables

Structure the requirement analysis as follows:

```
# <Feature Title> Feature Test Plan

## Requirements Analysis

| Scenario Name | Requirement | Test Case |
|---------------|-------------|-----------|
| Scenario <N>: <Name> | <N>.<M> <Requirement description> | <N>.<M>.<P> <Test case description> |
|  | <N>.<M> <Next requirement> | <N>.<M>.<P> <Test case description> |
|  |  | <N>.<M>.<P> <Additional test case> |
|
| Scenario <N+1>: <Name> | ... | ... |

## Detailed Test Cases

| Test Case ID | Test Scenario | Expected Result | Test Data |
|--------------|---------------|-----------------|-----------|
| <N>.<M>.<P> | <Action to perform> | <Expected outcome> | <Required data or N/A> |

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
- **Total Test Cases**: Count of all test case IDs defined (x.y.z format)
- **CSV Test Cases**: Count of actual test cases in the CSV file (may be less if test case IDs were combined)

**Key principles for requirement analysis:**

- Extract one requirement per testable aspect of the scenario
- Each requirement should map to one or more test cases
- Combine related requirements into single test cases when possible (minimize test case count)
- Use empty rows with `|` to visually separate scenarios
- Number requirements hierarchically: `<Scenario>.<Requirement>.<TestCase>`
- Always include the Test Coverage Summary section at the end for metrics tracking

### 2. Test Cases CSV File

**Location:** `docs/test-cases/`
**Naming:** `<UserStoryID>_<title>.TestCases.csv`
**Format:** Azure DevOps-compatible CSV

**CSV Structure:**

```csv
ID,Work Item Type,Title,Test Step,Step Action,Step Expected
,Test Case,<Test Case Title>,1,<Action>,<Expected Result>
,,,1,<Action>,<Expected Result>
```

**CRITICAL Azure DevOps CSV Rules:**

1. **First step MUST be duplicated**: The first test step MUST appear twice in consecutive rows with identical content. This is MANDATORY for Azure DevOps import.
2. **Empty first column**: ID column is always empty (Azure DevOps auto-generates)
3. **Empty rows between test cases**: Add one blank row between test cases for separation
4. **Multi-step format**: Continue steps as `2, 3, 4...` after the duplicated first step

**⚠️ CRITICAL: Every test case MUST have its first step duplicated on consecutive rows. This is required by Azure DevOps for proper test case import. Without this duplication, the import will fail or produce incorrect results.**

**Example CSV pattern:**

```csv
ID,Work Item Type,Title,Test Step,Step Action,Step Expected
,Test Case,Verify Login Screen Display,1,Click on the link to eNOW,Login screen is displayed
,,,1,Click on the link to eNOW,Login screen is displayed
,,,2,Verify all login elements are visible,Username field and password field are visible

,Test Case,Verify Email Format Validation,1,Enter invalid email format,Validation message appears
,,,1,Enter invalid email format,Validation message appears
,,,2,Observe validation message,Message says "Please enter a valid email address"
,,,3,Enter valid email format,Validation message disappears
```

**Note:** The first step (Step 1) is duplicated in rows 2 and 3 of each test case. This is mandatory.

## Test Case Design Principles

### Requirement Combination Strategy

**Goal**: Minimize total test case count while maintaining comprehensive coverage

**Combine requirements when:**

- They test the same UI component or screen
- They follow a natural sequential flow
- They share the same test setup/preconditions
- They logically belong to the same user action

**Example of effective combining:**
Instead of separate test cases for:

- "Verify Submit button exists"
- "Verify Submit button has correct color"
- "Verify Submit button is enabled by default"

Create one test case:

- "Verify Submit Button Properties" with multiple steps checking existence, color, and state

**Keep requirements separate when:**

- They test different user flows or paths
- They require different test data or preconditions
- Failure of one should not block testing of others
- They represent distinct acceptance criteria

### Test Step Guidelines

1. **Action clarity**: Each step should describe one clear action
2. **Expected results**: Be specific about what constitutes success
3. **Sequential flow**: Steps should follow natural user workflow
4. **Observability**: Expected results should be verifiable by the tester
5. **Completeness**: Include all necessary steps to validate the requirement

## Execution Steps

When invoked with a user story file:

1. **Read the user story file** from `docs/user-stories/`

   - Validate Gherkin format
   - If format issues exist, prompt for clarification or use `@bdd` skill

2. **Perform requirement analysis**

   - For each scenario, identify testable requirements
   - Map each requirement to test case(s)
   - Combine requirements into test cases where logical
   - Number everything hierarchically

3. **Create requirement analysis file**

   - Use the table format specified above
   - Include Requirements Analysis table with all scenarios, requirements, and test case IDs
   - Include Detailed Test Cases table with test case details
   - Add Test Coverage Summary section with metrics (Total Scenarios, Total Requirements, Total Test Cases, CSV Test Cases)
   - Save to `docs/requirement-analysis/<UserStoryID>_<title>.RequirementAnalysis.txt`
   - Keep analysis concise but complete

4. **Generate test cases CSV**

   - Convert requirement analysis into detailed test steps
   - **CRITICAL**: Apply Azure DevOps CSV formatting - duplicate the first test step for each test case
   - Add blank rows between test cases
   - Save to `docs/test-cases/<UserStoryID>_<title>.TestCases.csv`

5. **Validate outputs**
   - Confirm all scenarios have corresponding test cases
   - Verify CSV formatting follows Azure DevOps rules
   - Check that test case naming matches input file naming

## Quality Checks

Before finalizing outputs:

- [ ] All Gherkin scenarios are represented in requirement analysis
- [ ] Requirements are numbered hierarchically (Scenario.Requirement.TestCase)
- [ ] Test cases combine requirements effectively (not one test per requirement)
- [ ] Test Coverage Summary section is included with all metrics calculated
- [ ] CSV has duplicated first step for EVERY test case (mandatory Azure DevOps requirement)
- [ ] CSV has blank rows between test cases
- [ ] File naming matches input file naming convention
- [ ] Both output files are created in correct directories

## Error Handling

**Ambiguous requirements:**

- Prompt user for clarification with specific questions
- Do not make assumptions about unclear acceptance criteria

**Missing Gherkin format:**

- Alert user that Gherkin format is required
- Offer to help convert using `@bdd` skill

**Incomplete scenarios:**

- Identify what information is missing
- Request specific details needed to create test cases

## Example Reference

See the following example files for reference:

- Input: `docs/user-stories/eNr_118556_loading_screen.us.txt`
- Output 1: `docs/requirement-analysis/eNr_118556_loading_screen.RequirementAnalysis.txt`
- Output 2: `docs/test-cases/eNr_118556_loading_screen.TestCases.csv`

These files demonstrate the expected format and quality standards for all outputs.
