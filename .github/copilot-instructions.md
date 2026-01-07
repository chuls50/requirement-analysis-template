# AI Coding Agent Instructions

## Project Overview

This is a **requirement analysis template** that transforms Azure DevOps user stories (written in Gherkin/BDD format) into comprehensive test documentation. The workflow combines Python automation for Azure DevOps API integration with Claude AI skills for intelligent test case generation.

**Core workflow:** Azure DevOps User Story → Gherkin Acceptance Criteria → Requirement Analysis → Test Cases CSV (Azure DevOps-ready)

## Architecture & Key Components

### Directory Structure

```
.claude/skills/          # Claude AI skills (custom prompts/workflows)
├── export-user-story/   # Azure DevOps API integration skill
├── testcase-creator/    # Test documentation generation skill
└── skill-creator/       # Skill development framework

docs/                    # All generated documentation outputs
├── user-stories/        # *.us.txt - Exported Gherkin scenarios
├── requirement-analysis/# *.RequirementAnalysis.txt - Scenario-to-test mapping
└── test-cases/          # *.TestCases.csv - Azure DevOps import-ready

src/                     # Python scripts
└── export-acceptance-criteria.py  # Azure DevOps API client
```

### File Naming Convention

**Critical pattern:** `{PRODUCT_PREFIX}_{WORK_ITEM_ID}_{title_suffix}.{type}.{ext}`

Examples:
- `eNr_122019_participants_tab.us.txt` (user story)
- `eNr_122019_participants_tab.RequirementAnalysis.txt` (analysis)
- `eNr_122019_participants_tab.TestCases.csv` (test cases)

**Title extraction:** Everything after the colon in Azure DevOps title is lowercased and spaces become underscores. Example: "eNcounter Refresh: User Directory - Participants Tab" → `user_directory_-_participants_tab`

## Python Environment Setup

### Prerequisites
- Python 3.6+ required
- Uses virtual environment (`.venv/`) - **always activate before running scripts**
- Dependencies: `requests`, `python-dotenv`

### Configuration (CRITICAL)

Azure DevOps credentials are stored in `src/.env` (NOT committed to git):

```bash
AZURE_DEVOPS_ORGANIZATION=globalmeddev
AZURE_DEVOPS_PROJECT=GlobalMed
AZURE_DEVOPS_PAT=your-personal-access-token-here
PRODUCT_PREFIX=eNr
```

**Template:** `src/.env.example` shows required format

### Running Python Scripts

```powershell
# Activate virtual environment first
.\.venv\Scripts\Activate.ps1

# Then run script
python src/export-acceptance-criteria.py
```

## Key Workflows

### Workflow 1: Export User Story from Azure DevOps

**Using Claude skill:** `@export-user-story 122019 --full` (generates all outputs in one go)

**Manual Python approach:**
```powershell
cd src
python export-acceptance-criteria.py
# Enter work item ID when prompted
```

**What it does:**
1. Fetches work item via Azure DevOps REST API
2. Extracts `Microsoft.VSTS.Common.AcceptanceCriteria` field
3. Converts HTML to clean Gherkin format (removes tags, preserves bullets)
4. Ensures proper Gherkin formatting (Scenario/Given/When/Then on separate lines)
5. Saves to `docs/user-stories/` with standardized filename

### Workflow 2: Generate Test Documentation

**Using Claude skill:** `@testcase-creator generate test cases for eNr_122019_participants_tab.us.txt`

**Outputs:**
- `*.RequirementAnalysis.txt` - Hierarchical mapping: Scenario → Requirement → Test Case
- `*.TestCases.csv` - Azure DevOps Test Plans import format

## Document Format Conventions

### User Story Format (*.us.txt)

```
User Story ID: 121265
Title: eNcounter Refresh: Workspace- Screen Share
Type: User Story
==================================================

ACCEPTANCE CRITERIA:

Scenario 1: Screen Share Button
Given the eNcounter user has navigated to the workspace page 
When the page renders
Then the user will have the ability to select the screen share icon
```

**Critical:** Gherkin BDD format with proper Given/When/Then structure

### Requirement Analysis Format

Table-based hierarchical mapping showing traceability from scenarios through requirements to test cases. See [docs/requirement-analysis/eNr_121265_workspace-_screen_share.RequirementAnalysis.txt](../docs/requirement-analysis/eNr_121265_workspace-_screen_share.RequirementAnalysis.txt) for complete example.

### Test Case CSV Format

Azure DevOps Test Plans import-ready CSV with specific column structure. Must follow Azure DevOps naming and format requirements for successful import.

## Development Patterns

### HTML Processing in export-acceptance-criteria.py

**Critical transformation logic:** Lines 69-96 handle HTML-to-Gherkin conversion:
- `<br>` tags → newlines
- `<li>` tags → markdown bullets (`- `)
- HTML entities decoded (`&nbsp;`, `&amp;`, etc.)
- Ensures `Scenario`, `Given`, `When`, `Then` start on fresh lines
- Removes excessive blank lines while preserving structure

### Filename Sanitization

`sanitize_filename()` function (lines 18-22): Removes special chars, converts spaces to underscores, lowercases everything. **Never modify this logic** - it ensures consistency across all generated files.

## Testing & Quality Conventions

**Test case combination philosophy:** The `testcase-creator` skill follows a "smart combination" strategy:
- ✅ Combines related UI verifications into comprehensive test cases
- ✅ Keeps separate test cases for different user paths/workflows
- ✅ Independent error condition testing
- ✅ Minimizes test case count without sacrificing coverage

**Goal:** Efficient, maintainable test suites that reduce execution time while maintaining complete coverage.

## Common Tasks

### Adding support for new Azure DevOps fields
Modify `extract_acceptance_criteria()` in `export-acceptance-criteria.py` to fetch additional fields from the `fields` dictionary.

### Changing filename patterns
Update `generate_filename()` and `extract_title_suffix()` functions. **Warning:** This will break compatibility with existing Claude skills that expect the current naming convention.

### Extending test case generation
Claude skills in `.claude/skills/testcase-creator/` contain the prompts and logic for test generation. Modify skill definitions there, not in Python code.
