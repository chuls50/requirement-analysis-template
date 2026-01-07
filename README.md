# Test Case Creator Skill

> **Requirements-based test case generation from Gherkin user stories**

This repository contains a Claude Skill for automated test case creation using Behavior-Driven Development (BDD) methodology. The skill analyzes user story acceptance criteria written in **Gherkin syntax** and generates comprehensive test cases in Azure DevOps-compatible CSV format.

## ⚠️ Gherkin Syntax Required

This skill is designed to work with user stories that follow **Gherkin syntax** (Given/When/Then format). Scenarios must be structured using BDD conventions for optimal results.

## What This Skill Does

The `testcase-creator` skill transforms Gherkin user stories into two key outputs:

1. **Requirement Analysis Document** - Detailed breakdown of scenarios, requirements, and test case mapping
2. **Azure DevOps CSV Test Cases** - Import-ready test cases with proper formatting for Azure DevOps Test Plans

### Key Features

- ✅ Automatic requirement extraction from Gherkin scenarios
- ✅ Intelligent test case combination to minimize redundancy
- ✅ Azure DevOps CSV format compliance (including critical formatting rules)
- ✅ Hierarchical traceability (Scenario → Requirement → Test Case)
- ✅ Follows BDD and requirements-based testing methodology

## Quick Start

### Usage

Invoke the skill in Claude Desktop or VS Code with:

```
@testcase-creator generate test cases for <user_story_file>.us.txt
```

**Example:**

```
@testcase-creator generate test cases for eNr_118556_loading_screen.us.txt
```

### Input Format

User story files should be located in `docs/user-stories/` and follow this naming convention:

```
<UserStoryID>_<descriptive_title>.us.txt
```

**Example user story structure:**

```
User Story ID: 118556
Title: eNcounter Refresh: Loading Screen
Type: User Story
==================================================

ACCEPTANCE CRITERIA:

Scenario 1: Application loading screen is displayed
Given a user launches eNcounter from the desktop shortcut
When the application window opens
Then the loading screen is visible to the user

Scenario 2: Application loading screen completes successfully
Given eNcounter is loading at start up
When the loading process completes successfully
Then the application navigates to the Welcome/Log In screen
```

### Output Files

The skill generates two files with matching naming conventions:

1. **Requirement Analysis:**

   ```
   docs/requirement-analysis/<UserStoryID>_<title>.RequirementAnalysis.txt
   ```

2. **Test Cases CSV:**
   ```
   docs/test-cases/<UserStoryID>_<title>.TestCases.csv
   ```

## File Structure

```
skill-setup/
├── .claude/
│   └── skills/
│       ├── testcase-creator/           # Main skill
│       │   ├── SKILL.md               # Skill definition and workflow
│       │   └── references/
│       │       ├── azure-devops-csv-format.md      # CSV format specifications
│       │       └── requirement-analysis-guide.md   # Methodology & best practices
│       ├── skill-creator/             # Skill development framework
│       └── bdd/                       # BDD/Gherkin reference skill
├── docs/
│   ├── user-stories/                  # Input: Gherkin user stories
│   ├── requirement-analysis/          # Output: Requirement analysis documents
│   └── test-cases/                    # Output: Azure DevOps CSV test cases
└── testcase-creator.skill             # Packaged skill file
```

## Detailed Documentation

For comprehensive guidance on the methodology and formats used by this skill, see:

- **[Azure DevOps CSV Format](.claude/skills/testcase-creator/references/azure-devops-csv-format.md)** - Detailed specifications for Azure DevOps CSV import format, including critical formatting rules (duplicated first step, blank row separators)
- **[Requirement Analysis Guide](.claude/skills/testcase-creator/references/requirement-analysis-guide.md)** - Complete methodology for extracting requirements from Gherkin scenarios, combining test cases effectively, and maintaining traceability

## Example Workflow

1. **Create user story** with Gherkin scenarios in `docs/user-stories/`
2. **Invoke skill:** `@testcase-creator generate test cases for <filename>.us.txt`
3. **Review outputs:**
   - Requirement analysis in `docs/requirement-analysis/`
   - CSV test cases in `docs/test-cases/`
4. **Import to Azure DevOps** - CSV files are ready for direct import

## Examples

This repository includes complete examples demonstrating the skill in action:

- **[eNr_118556_loading_screen](docs/user-stories/eNr_118556_loading_screen.us.txt)** - Simple 3-scenario user story
- **[eNr_118557_welcomelog_in_screen](docs/user-stories/eNr_118557_welcomelog_in_screen_-_forms_based_authentication.us.txt)** - Complex 11-scenario authentication flow
- **[eNr_118559_select_patient](docs/user-stories/eNr_118559_select_patient_-_encounter_cloud_search.us.txt)** - Advanced 18-scenario patient search with validation

Each example includes the generated requirement analysis and test case CSV files.

## Test Case Design Philosophy

The skill follows a **smart combination strategy**:

- ✅ Combines related UI verifications into comprehensive test cases
- ✅ Keeps separate test cases for different user paths/workflows
- ✅ Validates error conditions independently from success paths
- ✅ Minimizes test case count while maintaining complete coverage

**Result:** Efficient, maintainable test suites that reduce execution time without sacrificing quality.

## Requirements

- Claude Desktop or VS Code with Claude integration
- Gherkin-formatted user stories
- Understanding of BDD methodology (recommended)

## Skills Included

- **testcase-creator** - Main test case generation skill
- **skill-creator** - Framework for creating and packaging skills
- **bdd** - Gherkin syntax and BDD reference skill

## Installation

1. Clone this repository
2. Import `testcase-creator.skill` into Claude Desktop
3. Place user stories in `docs/user-stories/`
4. Invoke the skill as shown in Quick Start

---

**Built with Claude Skills** | Designed for efficient requirements-based testing
