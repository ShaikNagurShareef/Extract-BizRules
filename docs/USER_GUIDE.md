# Business Rules Generation Skill - Complete User Guide

**This guide helps you use the skill to extract whatever you need from vendor layouts and source code.**

## Table of Contents
1. [Quick Start (5 min)](#quick-start)
2. [Available Workflows](#available-workflows)
3. [Detailed Examples](#detailed-examples)
4. [Q&A from Source](#qa-from-source)
5. [Specification Generation](#specification-generation)
6. [Troubleshooting](#troubleshooting)

---

## Quick Start

### Step 1: Gather Your Vendor Information

You need:
- **Vendor Name**: AnthemWGS_Claims
- **Domain**: Claims / Eligibility / Accumulators / RxClaims / PriorAuth
- **Layout File Path**: `/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx`
- **Codebase Root**: `{PROJECT_ROOT}/Codebase/`
- **C# Path**: `AccumulatorExtractor/.../PbmRecords/AnthemWgs/*.cs`
- **SQL Path**: `DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/`

### Step 2: Invoke the Skill

```
/generate-business-rules
vendor_name: AnthemWGS_Claims
domain_type: Claims
layout_file_path: /HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: AccumulatorExtractor/.../AnthemWgs/*.cs
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
```

### Step 3: Wait ~10 minutes

The skill processes in parallel:
```
🔄 Layout Analyzer: Extracting fields from Excel
🔄 Code Tracer: Finding C# properties
🔄 SQL Validator: Finding SQL transformations
...
✓ COMPLETE: Business Rules ready in /HRP Delivery/[DATE]/[Vendor]/
```

---

## Available Workflows

The skill supports different workflows depending on what you need:

### Workflow 1: Complete Business Rules (Default)
**What You Get**: Full Business Rules documentation with all mappings

**Time**: 10-15 minutes per vendor

**Output**:
- ✓ Business_Rules_[VendorName].xlsx (6 sheets)
- ✓ Layout Verification Report
- ✓ README documentation
- ✓ Ready for HRP implementation

**Use When**: You need complete documentation for implementation

**Example**:
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
domain_type: Claims
layout_file_path: ...
codebase_root: ...
```

---

### Workflow 2: Field Specification Only
**What You Get**: Just the field definitions and positions

**Time**: 5-7 minutes

**Output**:
- ✓ CSV/Excel with field specs (position, type, length, required)
- ✓ No code mapping (faster)
- ✓ Good for documentation

**Use When**: You just need to understand the layout structure

**Example**:
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: field-specs-only
layout_file_path: ...
output_format: csv  # or xlsx
```

---

### Workflow 3: Code Mapping Only
**What You Get**: Which C# properties map to which layout fields

**Time**: 8-10 minutes

**Output**:
- ✓ List of fields with their code properties
- ✓ Code locations (file + line)
- ✓ Transformation types
- ✓ No Excel file (just report)

**Use When**: You're debugging code and need to find where fields are used

**Example**:
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: code-mapping-only
codebase_root: ...
c_sharp_extractor_path: ...
output_format: markdown  # or csv
```

---

### Workflow 4: Transformation Logic Analysis
**What You Get**: How each field is transformed (SQL + C# logic)

**Time**: 10-12 minutes

**Output**:
- ✓ Transformation type for each field
- ✓ SQL stored procedure references
- ✓ Hard-coded values documented
- ✓ Business logic explained

**Use When**: You're implementing HRP logic and need to know transformations

**Example**:
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: transformations-only
codebase_root: ...
sql_root_path: ...
include_hard_coded_values: true
```

---

### Workflow 5: Q&A from Source (Interactive)
**What You Get**: Ask questions about the vendor, get answers from source

**Time**: Real-time responses

**Output**:
- ✓ Answers backed by actual code/layout
- ✓ Code location references
- ✓ Examples when applicable

**Use When**: You have specific questions about how something works

**Example**:
```
/q-and-a
vendor_name: AnthemWGS_Claims
codebase_root: ...
layout_file_path: ...

Questions:
1. How is GroupNumber calculated?
2. Which fields are hard-coded?
3. What transformations use crosswalks?
4. Where is validation logic?
```

---

### Workflow 6: Specification Generation
**What You Get**: Technical specification document

**Time**: 12-15 minutes

**Output**:
- ✓ Markdown/Word specification document
- ✓ Overview of vendor
- ✓ Field specifications table
- ✓ Transformation rules section
- ✓ Validation rules section
- ✓ Data quality rules

**Use When**: You need a formal specification document

**Example**:
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: specification-generation
layout_file_path: ...
codebase_root: ...
sql_root_path: ...
output_format: markdown  # or docx
spec_type: technical  # or business
```

---

## Detailed Examples

### Example 1: I Need to Understand a Vendor Layout

**Goal**: Get a quick overview of what fields a vendor provides

**Steps**:
1. Use Workflow 1 (Complete) OR Workflow 2 (Field Specs Only)
2. Field Specs is faster if you just need positions/types

**Command**:
```
/generate-business-rules
vendor_name: Navitus_Eligibility
workflow: field-specs-only
layout_file_path: /HRP/Navitus_Eligibility/OriginalVendorLayouts/MappingTemplate_*.xlsx
```

**Output**: CSV with columns: Position | Type | Length | Required | Notes

---

### Example 2: I Need to Implement Extract Logic in HRP

**Goal**: Get complete information to build the extract

**Steps**:
1. Use Workflow 1 (Complete Business Rules)
2. Review the Excel file with all mappings
3. Check README for explanations

**Command**:
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
domain_type: Claims
layout_file_path: /HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: AccumulatorExtractor/.../AnthemWgs/*.cs
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
```

**Next Steps**:
1. Open Business_Rules_AnthemWGS_Claims.xlsx
2. Review "Transformation Rules Summary" sheet
3. Check field mappings with code locations
4. Review README for how to use each field

---

### Example 3: A Field Is Missing from Code

**Goal**: Figure out why a layout field isn't in the C# code

**Steps**:
1. Generate Business Rules
2. Look for "Unmapped Fields" section
3. Use Q&A workflow to ask about it

**Command 1** (Find unmapped fields):
```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: code-mapping-only
codebase_root: ...
output_format: csv
```

**Command 2** (Ask about specific field):
```
/q-and-a
vendor_name: AnthemWGS_Claims
question: Why is MemberAltID not in the C# code?
codebase_root: ...
```

**Answer will show**:
- Is it truly missing?
- Is it named differently?
- Is it calculated from other fields?
- Is it not implemented yet?

---

### Example 4: I Need a Formal Specification

**Goal**: Create a document I can give to the business team

**Steps**:
1. Use Workflow 6 (Specification Generation)
2. Choose output format (Markdown or Word)
3. Review and edit if needed

**Command**:
```
/generate-business-rules
vendor_name: BCBSAZJA_Claims
workflow: specification-generation
layout_file_path: /HRP/BCBSAZJA_Claims/OriginalVendorLayouts/*.xlsx
codebase_root: ...
sql_root_path: ...
output_format: docx
spec_type: business
```

**Output**: Word document with:
- Executive Summary
- Overview
- Field Specifications Table
- Transformation Rules
- Validation & Quality Rules
- Appendices

---

### Example 5: Process Multiple Vendors at Once

**Goal**: Generate Business Rules for 5 vendors in parallel

**Steps**:
1. List all vendors
2. Provide shared codebase path
3. Skill processes all in parallel

**Command**:
```
/generate-business-rules
vendors:
  - vendor_name: AnthemWGS_Claims
    layout_file_path: /HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
  - vendor_name: Navitus_Eligibility
    layout_file_path: /HRP/Navitus_Eligibility/OriginalVendorLayouts/MappingTemplate_*.xlsx
  - vendor_name: BCBSAZJA_Claims
    layout_file_path: /HRP/BCBSAZJA_Claims/OriginalVendorLayouts/BCBSAZJA_Mapping.xlsx
  - vendor_name: ArchimedesRX_Accums-Medical
    layout_file_path: /HRP/ArchimedesRX_Accums-Medical/OriginalVendorLayouts/*.xlsx
  - vendor_name: MediKeeper_Claims
    layout_file_path: /HRP/MediKeeper_Claims/OriginalVendorLayouts/Medikeeper_Mapping_*.xlsx

codebase_root: {PROJECT_ROOT}/Codebase/
batch_output_directory: /HRP Delivery/07-28-2026/
```

**Result**: All 5 vendors processed in ~25 minutes (vs 50+ if sequential)

---

## Q&A from Source

You can ask the skill questions about any vendor, and it will search the source code and layout to find answers.

### How to Use Q&A

```
/q-and-a
vendor_name: AnthemWGS_Claims
codebase_root: {PROJECT_ROOT}/Codebase/
layout_file_path: /HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/

Your Questions:
1. How is GroupNumber mapped?
2. What transformations use the GroupNumber crosswalk?
3. Are there any hard-coded fields?
4. Which fields are required?
5. How is ClaimID validated?
```

### Example Questions

**Q: How is field X calculated?**
```
Q: How is GroupNumber mapped from the layout to the code?
A: GroupNumber is mapped via crosswalk lookup on Person.ParentPlan.PlanNum.
   Code: AnthemWgsDetailRecord.cs:36-41
   Example: PlanNum 0124012 → GroupNumber PPOLH
```

**Q: Which fields are hard-coded?**
```
Q: What fields are hard-coded in the AnthemWGS extract?
A: Found 3 hard-coded fields:
   1. AdjustmentType = "MED" (line 95) - Reason: Vendor only processes medical
   2. ProcessingType = "EDI" (line 103) - Reason: All claims via EDI
   3. RecordType = "D" (line 112) - Reason: Standard detail record marker
```

**Q: Where is validation for field X?**
```
Q: Where is validation for ClaimID?
A: Validation in 2 places:
   1. Code: AnthemWgsDetailRecord.cs:50 - Length check (20 chars)
   2. SQL: StoredProc_Validation.sql:145 - Uniqueness check
```

**Q: Which fields use this crosswalk?**
```
Q: Which fields use crosswalks?
A: Found 2 fields using crosswalks:
   1. GroupNumber - Uses GroupNumber_Crosswalk (line 36-41)
   2. CaseNumber - Uses CaseNumber_Mapping (line 87-93)
```

---

## Specification Generation

Generate formal technical or business specifications from the source.

### Technical Specification

```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: specification-generation
spec_type: technical
layout_file_path: ...
codebase_root: ...
sql_root_path: ...
output_format: markdown
```

**Output includes**:
- Architecture overview
- Field specifications (table)
- Data types and validation
- Transformation logic details
- SQL procedure references
- Code locations
- Configuration examples

### Business Specification

```
/generate-business-rules
vendor_name: AnthemWGS_Claims
workflow: specification-generation
spec_type: business
layout_file_path: ...
output_format: docx
```

**Output includes**:
- Business overview
- Field descriptions (business language)
- Business rules (WHY each field)
- Validation rules (business perspective)
- Example data scenarios
- No technical implementation details

---

## Troubleshooting

### Problem: "Vendor not found"

**Solution**:
1. Check exact vendor folder name: `/HRP/[EXACT_NAME]/`
2. Confirm OriginalVendorLayouts/ subfolder exists
3. Try: `/q-and-a` to list available vendors

### Problem: "Layout file has unexpected format"

**Solution**:
1. Verify file is Excel (.xlsx, not .xls)
2. Check file is readable (not password protected)
3. Try: `/q-and-a` to analyze the layout structure

### Problem: "C# code not found"

**Solution**:
1. Verify codebase_root path is correct
2. Check c_sharp_extractor_path pattern matches vendor
3. Try: `/q-and-a` to search for vendor code

### Problem: "Processing is slow"

**Solution**:
1. If vendor has 100+ fields, processing takes 15-20 min (normal)
2. For speed, use `workflow: field-specs-only`
3. For parallel processing, use `batch_mode: true`

### Problem: "Some fields unmapped"

**Solution**:
1. This is normal - some fields may not be implemented yet
2. Check in final report for "Unmapped Fields" section
3. Use `/q-and-a` to ask why specific field is unmapped

---

## Tips & Best Practices

1. **Start with field-specs**: Understand the layout first before deep dive
2. **Use Q&A**: Ask specific questions before generating full spec
3. **Batch process**: Run multiple vendors together for efficiency
4. **Review output**: Spot-check Business Rules file before using
5. **Keep layouts**: Original layout files are source of truth

---

**Last Updated**: July 28, 2026  
**Version**: 1.0.0
