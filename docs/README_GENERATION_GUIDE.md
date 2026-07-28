# README Generation Guide - Comprehensive Documentation for All Artifacts

**Version**: 2.0.0 | **Complete Documentation Strategy**

---

## Overview

Every artifact produced by the Business Rules Generation skill comes with a comprehensive README that explains:
- What the artifact contains
- How to use it
- Key findings and important notes
- How it connects to other artifacts

**Goal**: Any team member should be able to understand and use any artifact without needing to ask questions.

---

## Master Document: DELIVERY_MANIFEST.md

**Location**: `/HRP Delivery/[DATE]/[VendorDomain]/DELIVERY_MANIFEST.md`

**Purpose**: Master navigation guide for entire vendor package

**Contents**:

```markdown
# [VendorName] Business Rules Delivery - [DATE]

## Package Overview
- Vendor: [VendorName]
- Domain: [Claims/Eligibility/Accumulators]
- Package Date: [DATE]
- Status: COMPLETE / REVIEW NEEDED

## What's Included in This Package

### 1. Original Vendor Layouts
- **Location**: `OriginalVendorLayouts/`
- **Purpose**: Source vendor mapping documents (unchanged)
- **Contents**: [List of files]
- **See Also**: `OriginalVendorLayouts/README.md`

### 2. Extract Criteria & Business Rules
- **Location**: `Extract Criteria & Business Rules/`
- **Purpose**: Complete Business Rules documentation in Excel format
- **Main File**: `Business_Rules_[VendorName].xlsx`
- **Sheets**: Executive Summary, Field Mapping, Hard-Coded Values, Lookups, Validation Rules, Code Locations, SQL Transformations
- **Supporting Docs**: Field_Mapping_Guide.md, Transformation_Rules.md, Business_Logic_Documentation.md
- **See Also**: `Extract Criteria & Business Rules/README.md`

### 3. Layout Verification
- **Location**: `Layout Verification/`
- **Purpose**: Validation reports showing field-to-code mapping accuracy
- **Main File**: `Validation_Report_[VendorName].xlsx`
- **Supporting Docs**: Layout_Discovery_Report.md, Layout_Analysis.md, Validation_Summary.md
- **See Also**: `Layout Verification/README.md`

### 4. Code Tracing
- **Location**: `Code_Tracing/`
- **Purpose**: C# property mappings with exact code locations
- **Files**: Code_Locations.md, Transformation_Implementation.md
- **See Also**: `Code_Tracing/README.md`

### 5. SQL Analysis
- **Location**: `SQL_Analysis/`
- **Purpose**: SQL procedures and transformation logic
- **Files**: SQL_Procedures.md, Transformation_Logic.md, Crosswalks_and_Lookups.md
- **See Also**: `SQL_Analysis/README.md`

## Quick Navigation

**I want to...**
- Understand what fields this vendor has → See `Extract Criteria & Business Rules/Business_Rules_[VendorName].xlsx` Sheet 1
- Know how fields map to C# code → See `Code_Tracing/Code_Locations.md`
- Understand transformation logic → See `Extract Criteria & Business Rules/Transformation_Rules.md`
- See hard-coded values and WHY → See `Extract Criteria & Business Rules/Business_Logic_Documentation.md`
- Understand client-specific variations → See `Layout Verification/Client_Variations.md` (if applicable)
- Know what validation rules apply → See `Business Rules` file, Validation Rules sheet

## Key Statistics

- **Total Fields**: [N]
- **Fields from Vendor Layout**: [N]
- **Fields Mapped to Code**: [N] ([X]%)
- **Unmapped Fields**: [N] (flagged for review)
- **Hard-Coded Values**: [N]
- **Lookup/Crosswalk Rules**: [N]
- **Client Variations**: [N clients: list]
- **Validation Rules**: [N]

## Validation Results

### Coverage
- Layout fields documented: 100%
- Code locations found: [X]%
- SQL procedures referenced: [X]%
- Validation rules identified: [X]%

### Discrepancies Found
- [List any field mismatches, gaps, or issues]

### Special Notes
- [Client variations, record-type differences, etc.]

## How This Package Was Created

This package was created using the automated Business Rules Generation Skill, which:

1. **Extracted fields** from `OriginalVendorLayouts/` Excel files
2. **Traced fields** to C# domain model properties
3. **Mapped transformations** in SQL stored procedures
4. **Documented hard-coded values** with business reasoning
5. **Generated Business Rules** in standardized Excel format
6. **Created comprehensive README** files for all artifacts

**Process Time**: ~[X] minutes
**Automated By**: Claude Code Business Rules Skill v2.0.0

## Files and What They Contain

| File/Folder | Purpose | Use When |
|---|---|---|
| OriginalVendorLayouts/ | Source vendor mapping files | You need the original layout structure |
| Business_Rules_[VendorName].xlsx | Master Business Rules document | You're implementing extraction logic |
| Field_Mapping_Guide.md | How fields were extracted | You want to understand methodology |
| Transformation_Rules.md | All transformation types | You need to understand field transformations |
| Business_Logic_Documentation.md | Hard-coded values with WHY | You need to understand business logic |
| Code_Locations.md | Where in C# code each field is | You're reviewing/maintaining code |
| SQL_Procedures.md | SQL transformations | You're debugging SQL logic |
| Crosswalks_and_Lookups.md | Lookup table definitions | You need lookup mapping details |
| Validation_Report_[VendorName].xlsx | Validation results | You're verifying accuracy |
| Layout_Discovery_Report.md | How multi-sheet layouts were parsed | You want to understand layout complexity |
| Client_Variations.md | Client-specific differences | You're handling multiple client scenarios |

## Implementing This Vendor

### Step 1: Read the Overview
→ Start here: Executive Summary in `Business_Rules_[VendorName].xlsx`

### Step 2: Understand Fields
→ Review: Field mapping sheet in Excel file

### Step 3: Find Code Locations
→ Check: `Code_Tracing/Code_Locations.md`

### Step 4: Understand Transformations
→ See: `Extract Criteria & Business Rules/Transformation_Rules.md`

### Step 5: Handle Hard-Coded Values
→ Review: `Extract Criteria & Business Rules/Business_Logic_Documentation.md`

### Step 6: Check SQL Logic
→ Review: `SQL_Analysis/Transformation_Logic.md`

### Step 7: Validate
→ Compare against: `Layout Verification/Validation_Report_[VendorName].xlsx`

## Questions?

- **"How do I use the Business Rules file?"** → See `Extract Criteria & Business Rules/README.md`
- **"What validation was performed?"** → See `Layout Verification/README.md`
- **"Where is field X in the code?"** → See `Code_Tracing/Code_Locations.md`
- **"How is field X transformed?"** → See `Extract Criteria & Business Rules/Transformation_Rules.md`
- **"Why is value Y hard-coded?"** → See `Extract Criteria & Business Rules/Business_Logic_Documentation.md`
- **"How are clients handled?"** → See `Layout Verification/Client_Variations.md`

## Package Quality Assurance

✓ All fields extracted from vendor layouts
✓ All code locations verified against source
✓ All SQL procedures documented
✓ All hard-coded values with business reasoning
✓ All client variations documented
✓ All README files created and reviewed
✓ Package ready for HRP implementation

---

**Package Version**: 1.0.0
**Generated**: [DATE] by Claude Code Skill v2.0.0
**For**: HRP Implementation Team
**Status**: COMPLETE and READY FOR USE
```

---

## README File: Extract Criteria & Business Rules/README.md

**Location**: `Extract Criteria & Business Rules/README.md`

**Purpose**: Guide to using the Business Rules Excel file

**Contents**:

```markdown
# Extract Criteria & Business Rules - [VendorName]

## What's in This Folder

- **Business_Rules_[VendorName].xlsx** - Master Business Rules documentation
- **Field_Mapping_Guide.md** - Explains how fields were extracted
- **Transformation_Rules.md** - All transformation types
- **Business_Logic_Documentation.md** - Hard-coded values and WHY

## Understanding the Excel File

### Sheet 1: Executive Summary
- Overview of entire package
- Key statistics (field count, code coverage, etc.)
- Client variations summary
- Important notes and warnings

### Sheet 2: Transformation Rules Summary (HRP View)
- High-level view for HRP teams
- Transformation types summary (1-to-1, Lookups, Derived, Hard-Coded)
- Field count by transformation type
- Key business rules

### Sheet 3+: Field Mapping (Domain-Specific)
For each domain (Claims, Eligibility, Accumulators, etc.):

| Column | Contents |
|--------|----------|
| Sequence | Field order (1, 2, 3...) |
| Layout Field Name | From vendor layout |
| Position | Column position in vendor file |
| Type | A/N/D (Alphanumeric/Numeric/Date) |
| Length | Character length |
| Required? | Y/N |
| Code Property | C# property name |
| Code File | Which .cs file |
| Code Line | Line number (exact location) |
| Transformation Type | 1-to-1, Lookup, Derived, Hard-Coded, Unmapped |
| Business Rule | WHY this field exists |
| SQL Logic | If SQL transformation applies |
| Hard-Coded Value | If value is fixed (and why) |
| Example | Sample transformation |
| Notes | Additional context |

**Example Row**:
```
Sequence: 1
Layout Field: ClaimID
Position: 1-20
Type: A
Length: 20
Required: Y
Code Property: ClaimID
Code File: AnthemWgsDetailRecord.cs
Code Line: 42
Transformation Type: 1-to-1
Business Rule: Unique claim identifier from vendor
SQL Logic: None (direct mapping)
Hard-Coded Value: None
Example: WGS123456 → WGS123456
Notes: Used as primary key
```

### Sheet: Hard-Coded Values
All values that are set to fixed values in code:

| Column | Contents |
|--------|----------|
| Code Property | Field name |
| Hard-Coded Value | What value is set |
| Always Applied? | Y/N (conditional?) |
| Business Reason | WHY it's hard-coded |
| Code Location | Where in code (class.method) |
| Applied When | Under what conditions |

**Example**:
```
Code Property: AdjustmentType
Hard-Coded Value: "MED"
Always Applied: Y
Business Reason: This vendor only processes medical claims
Code Location: AnthemWgsDetailRecord.cs:95
Applied When: Every record
```

### Sheet: Lookups & Crosswalks
All lookup/crosswalk rules:

| Column | Contents |
|--------|----------|
| Field Name | Which field uses this lookup |
| Lookup Name | Name of crosswalk table |
| Source Field | What field provides the lookup key |
| Target Field | What field receives the result |
| Lookup Table | DB table or reference |
| Example Source | Sample input value |
| Example Result | Sample output value |
| Notes | Any special handling |

**Example**:
```
Field Name: GroupNumber
Lookup Name: GroupNumber_Crosswalk
Source Field: Person.ParentPlan.PlanNum
Target Field: GroupNumber (WGS format)
Lookup Table: db_GroupNumber_Mapping
Example Source: 0124012
Example Result: PPOLH
Notes: Used for vendor grouping
```

### Sheet: Validation Rules
All validation criteria:

| Column | Contents |
|--------|----------|
| Field Name | Which field |
| Validation Rule | What's validated |
| Type | Required, Format, Range, etc. |
| Details | Specific validation |
| Error Handling | What happens if invalid |
| Notes | Additional context |

## How to Use This File

**Scenario 1: I need to extract fields for HRP**
1. Open Business_Rules_[VendorName].xlsx
2. Go to appropriate Field Mapping sheet (Claims/Eligibility/etc.)
3. For each field:
   - Read Layout Field Name (from vendor file)
   - Read Position and Length
   - Read Code Property (where it maps in code)
   - Check Transformation Type (how to transform)
   - See Example (for sample transformation)

**Scenario 2: I need to find hard-coded values**
1. Open Hard-Coded Values sheet
2. Review Business Reason for each value
3. Check Code Location for exact location
4. See Applied When for conditions

**Scenario 3: I need to understand a transformation**
1. Find field in Field Mapping sheet
2. Read Transformation Type
3. Check Example for sample
4. See Business Rule for reasoning
5. Check SQL Logic if applicable
6. See Code Location for implementation

**Scenario 4: I need lookup/crosswalk details**
1. Go to Lookups & Crosswalks sheet
2. Find your field
3. See Lookup Name and tables
4. Check Example Source/Result
5. Review Notes for special handling

## Important Notes

⚠️ **Field Coverage**: [X]% of layout fields found in code
- Missing fields: [list any unmapped fields]
- [Note: these may be implemented differently or not yet in code]

⚠️ **Client Variations**: This vendor has [N] client variations
- See `../Layout Verification/Client_Variations.md` for details

⚠️ **SQL Transformations**: See `SQL_Analysis/Transformation_Logic.md` for complex SQL

⚠️ **Code Locations**: All line numbers are accurate as of [DATE]
- If code has changed, line numbers may shift

## Next Steps

1. Review Executive Summary for overview
2. Understand transformation types (see Transformation_Rules.md)
3. Review Field Mapping sheet for your domain
4. Check Code Locations for implementation details
5. Validate hard-coded values match your understanding
6. Review validation rules
7. Compare with original vendor layout (see OriginalVendorLayouts/)

---

**File Version**: 1.0.0
**Generated**: [DATE]
**Vendor**: [VendorName]
**Domain**: [Domain]
```

---

## README File: Layout Verification/README.md

**Location**: `Layout Verification/README.md`

**Purpose**: Explain validation methodology and results

**Contents**:

```markdown
# Layout Verification & Validation Results

## What Validation Was Performed

This folder contains validation reports showing:
1. Field extraction accuracy from vendor layout files
2. Field-to-code mapping verification
3. Client variation identification and documentation
4. Discrepancy detection

## Files in This Folder

- **Validation_Report_[VendorName].xlsx** - Detailed validation results
- **Layout_Discovery_Report.md** - How multi-sheet layout files were discovered and classified
- **Layout_Analysis.md** - Layout structure analysis and findings
- **Validation_Summary.md** - Summary of validation results
- **Client_Variations.md** - All client-specific variations (if applicable)

## Validation Report (Excel File)

### Sheet: Layout Analysis
- Field name, position, type, length from vendor layout
- Whether field was found in code
- Code location (if found)
- Transformation type
- Validation status (✓ OK, ⚠ WARNING, ❌ ERROR)

### Sheet: Unmapped Fields (if any)
- Fields in vendor layout but NOT found in code
- Possible reasons
- Recommended actions

### Sheet: Code-Only Fields (if any)
- Fields in code but NOT in vendor layout
- Possible reasons
- Notes

### Sheet: Discrepancies
- Any mismatches between layout and code
- Position differences
- Length differences
- Type differences
- Explanations

### Sheet: Coverage Statistics
- Total fields: [N]
- Fields found in code: [N] ([X]%)
- Unmapped: [N]
- Field count matches: ✓ / ❌
- Coverage assessment: [PASS/REVIEW NEEDED]

## Layout Discovery Process

See **Layout_Discovery_Report.md** for details on how multi-sheet layout files were discovered and classified.

This report explains:
- How many sheets were in the layout file
- How each sheet was classified (Main Fields, Variations, Crosswalks, etc.)
- How client-specific variations were extracted
- How all data was consolidated into master field list

## Layout Analysis

See **Layout_Analysis.md** for detailed analysis of:
- Layout file structure
- Record types identified (Header, Detail, Trailer, etc.)
- Client variations found
- Field characteristics

## Validation Summary

See **Validation_Summary.md** for:
- Validation results overview
- Any gaps or discrepancies
- Field coverage statistics
- Recommendations for next steps

## Client Variations

If this vendor has client-specific variations, see **Client_Variations.md** which documents:
- All client names
- Fields that vary by client
- What changes for each client
- Examples of variations

## Quality Metrics

**Coverage**: [X]% of layout fields found in code
- Status: ✓ ACCEPTABLE / ⚠ NEEDS REVIEW

**Accuracy**: [X]% of code locations verified
- Status: ✓ ACCEPTABLE / ⚠ NEEDS REVIEW

**Completeness**: All required documentation generated
- Status: ✓ COMPLETE / ⚠ NEEDS REVIEW

**Overall Assessment**: [PASS / REVIEW NEEDED]

## How to Use These Reports

**To verify accuracy of field mappings**:
1. Open Validation_Report_[VendorName].xlsx
2. Go to Layout Analysis sheet
3. Review each row to see if field was found in code

**To find unmapped fields**:
1. Open Validation_Report_[VendorName].xlsx
2. Go to Unmapped Fields sheet (if present)
3. Review reasons and take recommended actions

**To understand client variations**:
1. Open Client_Variations.md
2. Review which fields vary by client
3. See examples of how fields differ

**To verify completeness**:
1. See Validation_Summary.md
2. Review coverage statistics
3. Check for any gaps or discrepancies

## Next Steps

1. ✓ Review validation summary
2. ✓ Check for unmapped fields (and decide what to do about them)
3. ✓ Review client variations (if applicable)
4. ✓ Verify code locations in Business Rules file
5. Proceed with implementation confidence

---

**Validation Date**: [DATE]
**Validation Status**: COMPLETE
```

---

## Additional README Files

### Code_Tracing/README.md
- Explains how code locations were identified
- How to read C# property names and line numbers
- How to find code in actual .cs files

### SQL_Analysis/README.md
- Explains how SQL procedures were located
- How transformation SQL was extracted
- How to read SQL transformation logic

### OriginalVendorLayouts/README.md
- Lists all original vendor mapping files
- Explains what each file contains
- Notes on file format and structure

---

## README Template Standards

All README files should:

✓ **Start with clear title** (what this folder/file is for)
✓ **Explain purpose** (why this artifact exists)
✓ **List contents** (what files are included)
✓ **Give usage examples** (how to use the artifact)
✓ **Show key information** (important findings, warnings)
✓ **Provide navigation** (links to related artifacts)
✓ **Answer common questions** ("How do I use this?", "What does this mean?")
✓ **End with next steps** (what to do after reviewing)

## Quality Checklist

For each README file, verify:
- [ ] Title is clear and descriptive
- [ ] Purpose is explicitly stated
- [ ] All files/folders listed with descriptions
- [ ] Examples provided (if applicable)
- [ ] Related artifacts referenced
- [ ] Important findings highlighted
- [ ] Navigation aids included (Table of Contents, links)
- [ ] Common questions answered
- [ ] No jargon without explanation
- [ ] Helpful for someone unfamiliar with the package

---

**README Guide Version**: 2.0.0
**Status**: Ready for skill implementation
**Priority**: HIGH - Every artifact must be fully documented
