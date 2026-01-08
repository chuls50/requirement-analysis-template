---
name: testcase-creator
description: Generate requirements-based test cases from Gherkin user stories using BDD methodology. Use this skill when the user asks to create test cases, generate test cases from user stories, analyze requirements for testing, convert user stories to test cases, or work with files in docs/user-stories/. This skill performs comprehensive requirement analysis and outputs Azure DevOps-compatible CSV test case files with proper formatting for import.
---

# Test Case Creator

Transform Gherkin user stories into comprehensive, requirements-based test cases through systematic requirement analysis.

## Input

**File location:** `docs/user-stories/<UserStoryID>_<title>.us.txt`  
**Format:** Gherkin scenarios (Given/When/Then syntax)

## Output

Two files with consistent naming:

1. **Requirement Analysis:** `docs/requirement-analysis/<UserStoryID>_<title>.RequirementAnalysis.txt`

   - Requirements Analysis table (Scenario → Requirement → Test Case mapping)
   - Detailed Test Cases table (one row per test case ID, hierarchical numbering x.y.z)
   - Test Coverage Summary

2. **Test Cases CSV:** `docs/test-cases/<UserStoryID>_<title>.TestCases.csv`
   - Azure DevOps import format
   - Columns: `ID,Work Item Type,Title,Test Step,Step Action,Step Expected`
   - First step MUST be duplicated for each test case
   - Blank row between test cases

## Critical Requirements

⚠️ **Detailed Test Cases table:**

- Use hierarchical IDs (1.1.1, 1.2.1, 2.1.1...) NOT TC-01, TC-02
- One row per test case ID (no combination)

⚠️ **CSV file:**

- Duplicate first step for EVERY test case (mandatory Azure DevOps requirement)
- Combine related test case IDs to minimize CSV row count

## Documentation

📚 **Comprehensive guides:**

- `references/requirement-analysis-guide.md` - Complete formatting rules, examples, anti-patterns
- `references/azure-devops-csv-format.md` - CSV format specifications

📁 **Working examples:**

- `examples/eNr_121265_workspace-_screen_share.*` - Complete input/output example set

## Execution

1. Read user story from `docs/user-stories/`
2. Extract requirements and map to test case IDs (hierarchical numbering)
3. Generate RequirementAnalysis.txt (follow `references/requirement-analysis-guide.md`)
4. Generate TestCases.csv (follow `references/azure-devops-csv-format.md`)
5. Verify: hierarchical numbering, duplicated first steps, blank rows between test cases
