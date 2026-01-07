---
name: export-user-story
description: Export user story acceptance criteria from Azure DevOps by ID and optionally generate requirement analysis and test cases. Use this skill when the user wants to export a user story from Azure DevOps, create test documentation, or generate a complete test package from a user story ID.
---

# Export User Story

Export user story acceptance criteria from Azure DevOps and optionally generate requirement analysis and test cases.

## Quick Start

**Export only:**
```
@export-user-story 122019
```

**Export + generate test cases:**
```
@export-user-story 122019 --full
```

## Workflow

### 1. Export Acceptance Criteria

Run the Python export script:

```bash
cd src
python export-acceptance-criteria.py
```

**What it does:**
- Fetches user story from Azure DevOps API
- Converts HTML to formatted text with:
  - Proper Scenario spacing
  - Bullet points as markdown (`- Item`)
  - Clean Given/When/Then formatting
- Saves to: `docs/user-stories/<prefix>_<id>_<title>.us.txt`

**Prerequisites:**
- `src/.env` configured with Azure DevOps credentials
- Python packages installed (see README)

### 2. Generate Test Cases (Optional)

After export completes, invoke the testcase-creator skill:

```
@testcase-creator generate test cases for <filename>.us.txt
```

**What it generates:**
- `docs/requirement-analysis/<filename>.RequirementAnalysis.txt` - Requirement breakdown and metrics
- `docs/test-cases/<filename>.TestCases.csv` - Azure DevOps importable test cases

## Full Workflow

When user requests `--full` or "complete test package":

1. **Prompt for User Story ID** if not provided
2. **Run export script** to fetch from Azure DevOps
3. **Invoke testcase-creator** skill automatically with the exported file
4. **Confirm outputs** created:
   - `.us.txt` (acceptance criteria)
   - `.RequirementAnalysis.txt` (requirement analysis)
   - `.TestCases.csv` (test cases)

## Configuration

The export script uses environment variables from `src/.env`:

```
AZURE_DEVOPS_ORGANIZATION=your-org
AZURE_DEVOPS_PROJECT=your-project
AZURE_DEVOPS_PAT=your-personal-access-token
PRODUCT_PREFIX=eNr
```

Copy `src/.env.example` to `src/.env` and fill in your values.

## File Naming Convention

All generated files follow the same naming pattern:

```
<prefix>_<id>_<title>.<extension>
```

Examples:
- `eNr_122019_participants_tab.us.txt`
- `eNr_122019_participants_tab.RequirementAnalysis.txt`
- `eNr_122019_participants_tab.TestCases.csv`

## Error Handling

**Authentication errors:**
- Verify PAT in `.env` has not expired
- Check organization and project names are correct

**Empty acceptance criteria:**
- Script will prompt whether to create empty file
- Manual entry may be needed in Azure DevOps

**Formatting issues:**
- Script handles HTML lists, line breaks, and special characters
- Scenarios automatically separated with blank lines
