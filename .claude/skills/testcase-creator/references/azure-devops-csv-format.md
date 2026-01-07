# Azure DevOps CSV Format Reference

This document provides detailed specifications for the Azure DevOps CSV test case import format.

## CSV Structure

```csv
ID,Work Item Type,Title,Test Step,Step Action,Step Expected
```

### Column Definitions

- **ID**: Always empty (Azure DevOps auto-generates work item IDs)
- **Work Item Type**: Always "Test Case"
- **Title**: Descriptive name of the test case
- **Test Step**: Sequential step number (1, 2, 3, etc.)
- **Step Action**: What the tester should do
- **Step Expected**: What should happen (expected result)

## Critical Formatting Rules

### 1. Duplicated First Step (MANDATORY)

**⚠️ CRITICAL REQUIREMENT: The first test step MUST appear twice in consecutive rows. This is an Azure DevOps import requirement and the import will fail or produce incorrect results if not followed.**

The first test step MUST be duplicated on consecutive rows with identical content in both the "Step Action" and "Step Expected" columns. This is not optional - it is required by Azure DevOps for proper test case import.

**Correct:**

```csv
,Test Case,Verify Login,1,Click login button,Login form appears
,,,1,Click login button,Login form appears
,,,2,Enter credentials,Fields accept input
```

**Incorrect:**

```csv
,Test Case,Verify Login,1,Click login button,Login form appears
,,,2,Enter credentials,Fields accept input
```

**Why This Matters:**
Azure DevOps requires the first step to be duplicated to properly parse and import test case steps. Without this duplication, the test case may import incorrectly or fail validation. **Always verify that every test case has its first step duplicated before importing to Azure DevOps.**

### 2. Empty Rows Between Test Cases

Add one blank row between test cases for visual separation and proper import.

**Correct:**

```csv
,Test Case,Test Case 1,1,Action,Result
,,,1,Action,Result

,Test Case,Test Case 2,1,Action,Result
,,,1,Action,Result
```

### 3. Subsequent Steps Format

After the duplicated first step, continue with steps 2, 3, 4, etc. Leave the first three columns empty.

**Example:**

```csv
ID,Work Item Type,Title,Test Step,Step Action,Step Expected
,Test Case,Verify Email Validation,1,Enter invalid email,System shows error
,,,1,Enter invalid email,System shows error
,,,2,Observe error message,Message says "Invalid email format"
,,,3,Enter valid email,Error message disappears
,,,4,Click submit,Form is submitted successfully
```

## Complete Example

```csv
ID,Work Item Type,Title,Test Step,Step Action,Step Expected
,Test Case,Verify Login Screen Display,1,Click on the link to eNOW,Login screen is displayed
,,,1,Click on the link to eNOW,Login screen is displayed

,Test Case,Verify Login Screen Elements,1,Observe the right half of the screen,Right half of screen displays a configurable image
,,,1,Observe the right half of the screen,Right half of screen displays a configurable image
,,,2,Observe the top left corner of the screen,Top left corner contains a language dropdown with all available languages
,,,3,Observe the screen label,Screen displays Login label
,,,4,Observe the text entry box label,Text entry box is labeled Email
,,,5,Observe the text entry box default content,Text entry box contains default text Enter email
,,,6,Observe the Next button,Next button is present with configurable color and is enabled by default

,Test Case,Verify Language Dropdown Display,1,Click on the language dropdown,List of available languages is displayed
,,,1,Click on the language dropdown,List of available languages is displayed

,Test Case,Verify Empty Email Field Validation,1,Leave email field empty,Email field contains no value
,,,1,Leave email field empty,Email field contains no value
,,,2,Click the Next button,Error message Enter an email is displayed
```

## Common Mistakes

### ❌ Missing duplicated first step

```csv
,Test Case,Test Name,1,Do something,Result happens
,,,2,Do next thing,Next result
```

### ❌ Not leaving columns empty for subsequent steps

```csv
,Test Case,Test Name,1,Action,Result
,Test Case,Test Name,2,Action,Result  # Wrong: should leave columns 2-3 empty
```

### ❌ No blank row between test cases

```csv
,Test Case,Test 1,1,Action,Result
,,,1,Action,Result
,Test Case,Test 2,1,Action,Result  # Missing blank row above
```

## Validation Checklist

Before finalizing CSV:

- [ ] Header row present with all 6 columns
- [ ] First column (ID) is empty for all rows
- [ ] Each test case has "Test Case" in Work Item Type column
- [ ] First step is duplicated (appears in rows 2 and 3 of each test case)
- [ ] Subsequent steps leave first 3 columns empty
- [ ] Blank row between each test case
- [ ] No trailing commas or malformed rows
