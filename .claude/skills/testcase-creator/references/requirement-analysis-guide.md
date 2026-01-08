# Requirement Analysis Guide

Extract requirements from Gherkin scenarios and map to test cases with hierarchical numbering.

## 4-Step Analysis Process

### 1. Parse Gherkin Scenarios

- **Given**: Preconditions, **When**: Actions, **Then**: Expected outcomes, **And**: Additional steps
- Analyze if multiple "And" clauses = separate requirements or combined verification

### 2. Extract Requirements

- Specific and measurable
- Starts with what "should" happen
- Avoid vague language ("works correctly")
- One testable aspect per requirement

### 3. Identify Combination Opportunities

**✅ Combine:** Natural workflow, redundant setup if separate, multi-aspect verification of same interaction, maintainable  
**❌ Separate:** Different paths, different preconditions, independent failures, error vs success conditions

### 4. Map to Test Cases

Hierarchical numbering: `Scenario.Requirement.TestCase` (e.g., 1.1.1, 1.2.1, 2.1.1)

## Output Format

### Requirements Analysis Table

Scenario → Requirement → Test Case traceability

**Format:**

- Empty cells (`|  |`) continue same scenario/requirement
- Empty row (`|`) separates scenarios
- Number hierarchically: Scenario (1, 2), Requirement (1.1, 1.2), Test Case (1.1.1, 1.1.2)

### Detailed Test Cases Table

**⚠️ CRITICAL: One row per test case ID (x.y.z) from Requirements Analysis**

**Common Mistakes:**

- ❌ TC-01, TC-02, TC-03 format
- ❌ Combining IDs into fewer rows
- ✅ Hierarchical IDs matching Requirements Analysis

**Columns:** Test Case ID | Test Scenario | Expected Result | Test Data

### Test Coverage Summary

**Metrics:**

- Total Scenarios (count)
- Total Requirements (x.y format count)
- Total Test Cases (x.y.z format count) = Detailed Test Cases row count
- CSV Test Cases (count, may be less due to CSV combination)

## Complete Examples

**📁 See `examples/` folder for full working examples:**

**eNr_121265 (Screen Share):** 13 scenarios → 43 requirements → 43 test case IDs → 13 CSV test cases  
**eNr_122019 (User Directory):** Tab navigation, filtering, user workflows

**Study examples for:** And clause analysis, combine vs separate decisions, hierarchical numbering, Requirements-to-CSV mapping

## Decision Guidelines

**Multiple "And" Clauses:**

- Same action/load verification? → Combine
- Independent features? → Separate
- Maintainable combined? → If yes, combine

**Conditional Scenarios:** Different preconditions (license types, roles, flags) → Separate

**Navigation:** Simple nav + destination → Combine | Complex multi-step → Break into checkpoints

## Anti-Patterns

❌ **Over-Granular:** Separate test per button property  
✅ **Better:** Group related verifications

❌ **Under-Specified:** "Should work correctly"  
✅ **Better:** "Should display validation error when email format is invalid"

❌ **Unrelated Combination:** Login + profile + logout in one test  
✅ **Better:** Separate workflows

❌ **Wrong IDs:** TC-01, TC-02 in Detailed Test Cases  
✅ **Better:** 1.1.1, 1.2.1, 2.1.1

## Quality Checklist

- [ ] All scenarios represented
- [ ] Requirements testable and specific
- [ ] Hierarchical numbering (x.y.z)
- [ ] Detailed Test Cases: one row per test case ID
- [ ] Total Test Cases count = Detailed Test Cases rows
- [ ] Tables formatted with empty cells/separators
