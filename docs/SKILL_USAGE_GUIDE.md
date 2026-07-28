# Business Rules Generation Skill - Usage Guide

## Quick Start (5 Minutes)

### 1. Gather Required Information

Before you invoke the skill, collect:

```
Vendor Name:        AnthemWGS_Claims
Domain Type:        Claims
Layout File Path:   {PROJECT_ROOT}/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
Codebase Root:      {PROJECT_ROOT}/Codebase/
C# Extractor Path:  AccumulatorExtractor/Accumulator Extractor.NET/AccumulatorExtractor.NET/PbmRecords/AnthemWgs/*.cs
SQL Root Path:      DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
```

### 2. Invoke the Skill

In Claude Code, type:

```
/generate-business-rules
vendor_name: AnthemWGS_Claims
domain_type: Claims
layout_file_path: {PROJECT_ROOT}/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: AccumulatorExtractor/Accumulator Extractor.NET/AccumulatorExtractor.NET/PbmRecords/AnthemWgs/*.cs
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
```

### 3. Wait for Pre-Flight Checks

The skill will display:

```
INPUT VALIDATION REPORT
✓ vendor_name: "AnthemWGS_Claims"
✓ layout_file_path: EXISTS and readable (1.2 MB)
✓ codebase_root: EXISTS with 45 subdirectories
✓ c_sharp_extractor_path: Found 3 .cs files (AnthemWgs*.cs)
✓ sql_root_path: EXISTS with 128 stored procedures

⚠ DEVIATION: Layout file has 145 fields (medium-sized vendor)
⚠ DEVIATION: Multiple client variations detected (7 sheets)

READY TO PROCEED? (y/n)
```

Type `y` to continue.

### 4. Monitor Progress

Watch real-time progress:

```
[PARALLEL AGENT EXECUTION] Status: RUNNING
  🔄 Layout Analyzer:     45% (Detail fields extracted) [3 min remaining]
  🔄 Code Tracer:         60% (Mapped 87/145 fields) [2 min remaining]
  🔄 SQL Validator:       40% (Found 3 transformation procedures) [4 min remaining]
  🔄 Business Logic:      35% (Extracted 12 hard-coded values) [5 min remaining]

[AGGREGATION & VALIDATION] Status: WAITING
  Waiting for all agents to complete...

[README GENERATOR] Status: PENDING
  Will start when aggregation complete...
```

### 5. Review Output

When complete:

```
✓ WORKFLOW COMPLETE - All agents finished successfully

RESULTS SUMMARY:
  • Total fields extracted: 145
  • Fields mapped to code: 141 (97%)
  • Unmapped fields: 4 (flagged)
  • Hard-coded values documented: 12
  • Business rules created: 145
  • README files generated: 2

DELIVERABLE PACKAGE:
  ✓ Business_Rules_AnthemWGS_Claims.xlsx (created)
  ✓ Layout Verification report (created)
  ✓ Extract Criteria & Business Rules/README.md (created)
  ✓ Layout Verification/README.md (created)

LOCATION:
  /HRP Delivery/07-28-2026/AnthemWGS_Claims/

NEXT STEPS:
  1. Review Business_Rules_AnthemWGS_Claims.xlsx
  2. Check README files for explanations
  3. Verify unmapped fields in final report
  4. Commit to GitHub if satisfied
```

---

## Step-by-Step Examples

### Example 1: Process a Single Small Vendor

**Goal**: Generate Business Rules for a small vendor (Navitus - ~80 fields)

**Command**:
```
/generate-business-rules
vendor_name: Navitus_Eligibility
domain_type: Eligibility
layout_file_path: {PROJECT_ROOT}/HRP/Navitus_Eligibility/OriginalVendorLayouts/MappingTemplate_Navitus_Health_Solutions_Standard_Eligibility_Member_Flat_File_V3_5.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: AvailabilityExtractor/src/PbmRecords/Navitus/*.cs
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
```

**Expected Time**: 8-10 minutes

**What You'll See**:
- Layout Analyzer extracts 80 fields
- Code Tracer finds mappings for ~75 fields
- 5 unmapped fields flagged
- Business Logic documents any hard-coded values
- Complete Excel file with 6 sheets

---

### Example 2: Process Multiple Vendors in Parallel

**Goal**: Generate Business Rules for 3 vendors simultaneously

**Command**:
```
/generate-business-rules
vendors:
  - vendor_name: AnthemWGS_Claims
    layout_file_path: {PROJECT_ROOT}/.../Anthem_WGS_835_Mapping.xlsx
    domain_type: Claims
  - vendor_name: Navitus_Eligibility
    layout_file_path: {PROJECT_ROOT}/.../Navitus_Eligibility_Mapping.xlsx
    domain_type: Eligibility
  - vendor_name: ArchimedesRX_Accums-Medical
    layout_file_path: {PROJECT_ROOT}/.../Archimedes_Mapping.xlsx
    domain_type: Accumulators
codebase_root: {PROJECT_ROOT}/Codebase/
```

**Expected Time**: ~25 minutes for all 3

**Progress Display**:
```
VENDOR 1 (AnthemWGS_Claims):     ████████░░ 80% (3 min remaining)
VENDOR 2 (Navitus_Eligibility):  ███░░░░░░░ 30% (8 min remaining)
VENDOR 3 (ArchimedesRX_Accums):  ░░░░░░░░░░  0% (waiting for V1/V2 to finish)

OVERALL ETA: 25 minutes
```

---

### Example 3: Handle a Large Vendor with Many Fields

**Goal**: Process BCBSAZJA (200+ fields) with client variations

**Command**:
```
/generate-business-rules
vendor_name: BCBSAZJA_Claims
domain_type: Claims
layout_file_path: {PROJECT_ROOT}/.../BCBSAZJA_Claims_Mapping.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: ClaimsExtractor/.../PbmRecords/BCBSAZJA/*.cs
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
client_variations: Y
```

**What Happens**:
- Skill detects 200+ fields
- Automatically segments into groups (Header: 10 fields, Detail: 180 fields, Trailer: 15 fields)
- Processes each segment in parallel for accuracy
- Documents all client variations (10+ different mapping variations)

**Expected Time**: 18-22 minutes

**Special Handling**:
```
⚠ DEVIATION: Large vendor detected (205 fields)
   Action: Segmenting into 3 groups for accurate processing
   - Group 1 (Header): 10 fields
   - Group 2 (Detail): 180 fields
   - Group 3 (Trailer): 15 fields
   
⚠ DEVIATION: Client variations detected (10 sheets in layout)
   Action: Will document variations for each client
   Clients: ABA, ACBL, ALCA, ARSI, ASI, AUII, BCL, BJI, BLS, BYT
```

---

## Understanding the Output

### Business Rules Excel File

**File**: `Business_Rules_[VendorName].xlsx`

**Sheets**:

1. **Executive Summary**
   - Package name and vendor
   - Total fields: 145
   - Fields mapped: 141 (97%)
   - Unmapped fields: 4
   - Client variations: 7
   - Hard-coded values: 12
   - How to use this file

2. **Transformation Rules Summary** (HRP View)
   - 1-to-1 Mapping: 120 fields
   - Derived Fields: 10 fields
   - Lookup/Crosswalk: 8 fields
   - Hard-Coded: 4 fields
   - Unmapped: 3 fields
   - Summary of key transformations

3. **Claims Field Mapping** (Detailed)
   | Sequence | Layout Field | Position | Type | Code Property | Transformation Type | Business Rule | Code Location | Example |
   |----------|--------------|----------|------|----------------|-------------------|-----------------|------------------|---------|
   | 1 | ClaimID | 1-20 | A | ClaimID | 1-to-1 | Unique claim identifier | AnthemWgsDetailRecord.cs:42 | WGS123456 → WGS123456 |
   | 2 | GroupNumber | 21-30 | A | GroupNumber | Lookup | Crosswalk lookup on PlanNum | AnthemWgsDetailRecord.cs:87 | 0124012 → PPOLH |

4. **Hard-Coded Values**
   | Code Property | Hard-Coded Value | Always Applied? | Business Reason | Code Location | Applied When |
   |----------------|-----------------|------------------|-----------------|----------------|--------------|
   | AdjustmentType | "MED" | Always | Vendor only processes medical claims | Line 95 | Every record |
   | ProcessingType | "EDI" | Always | Claims received via EDI format | Line 103 | Every record |

5. **Lookups & Crosswalks**
   - Crosswalk: GroupNumber
   - Source Field: Person.ParentPlan.PlanNum
   - Target Field: GroupNumber (WGS format)
   - Lookup Table: db_GroupNumber_Mapping
   - Example: 0124012 → PPOLH

6. **Validation Rules**
   - ClaimID: Must be 20 alphanumeric, unique
   - GroupNumber: Must exist in GroupNumber_Crosswalk
   - Amount fields: Numeric, precision 2 decimals

### Layout Verification Report

**File**: `Layout Verification/Validation_Report_[VendorName].xlsx`

Contains:
- Field-by-field validation results
- Code location accuracy
- Transformation logic verification
- Any discrepancies or warnings

### README Files

**File 1**: `Extract Criteria & Business Rules/README.md`

Explains:
- What each sheet in Business Rules contains
- How to use the documentation
- Location of hard-coded values
- List of fields and brief descriptions
- How to implement each field in HRP

**File 2**: `Layout Verification/README.md`

Explains:
- What validation was performed
- Results summary
- Any fields needing special attention
- How to interpret the validation report

---

## Troubleshooting

### "vendor_name not recognized"

**Problem**: The vendor folder doesn't exist in HRP

**Solution**:
```
1. Check folder name exactly (case-sensitive on Mac/Linux)
2. Verify path: /HRP/[vendor_name]/ exists
3. Confirm OriginalVendorLayouts/ subdirectory is present
```

**Example Fix**:
```
Error: AnthemWGS (not found)
Correct: AnthemWGS_Claims or AnthemWGS_Eligibility
```

---

### "layout_file_path does not exist"

**Problem**: The layout Excel file path is wrong

**Solution**:
```
1. Verify exact file name in /OriginalVendorLayouts/
2. Use absolute path: /Users/284685/.../filename.xlsx
3. Check file extension (.xlsx not .xls)
```

**Example Fix**:
```
Wrong: /HRP/AnthemWGS_Claims/Anthem_Mapping.xlsx
Right: /HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
```

---

### "C# extractor files not found"

**Problem**: The skill can't find C# code files

**Solution**:
```
1. Verify codebase_root path is correct
2. Check c_sharp_extractor_path relative path
3. Look for pattern matching (*.cs files)
4. Vendor name must match class name pattern
```

**Example Fix**:
```
Looking for: AnthemWGS
Check these locations:
  /Codebase/AccumulatorExtractor/.../AnthemWgs*.cs
  /Codebase/ClaimsExtractor/.../AnthemWgs*.cs
```

---

### "Process is taking longer than expected"

**Problem**: Skill is running slowly

**Reason**: Your vendor probably has many fields (100+)

**Action**:
```
Let it continue. Large vendors (150+ fields) take 15-25 minutes.
The skill automatically segments for accuracy.
Check progress messages for current ETA.
```

---

### "Some fields unmapped - is this normal?"

**Problem**: Some layout fields not found in C# code

**Why This Happens**:
- Field exists in layout but not implemented yet
- Field is handled differently in code
- Field name differs from layout

**What Skill Does**:
- Lists all unmapped fields clearly
- Marks them in final report
- Flags for manual review

**Next Steps**:
1. Review unmapped fields list
2. Search code for similar field names
3. Manually map if field exists under different name
4. Confirm field is truly not implemented

---

### "Deviation detected - should I proceed?"

**Problem**: Skill detected something unexpected

**What to Do**:
```
Read the deviation message carefully:
  Issue: [What's different]
  Impact: [How it affects results]
  Action: [What skill will do]

Answer 'y' to continue unless impact is unacceptable
Answer 'n' to stop and adjust inputs
```

**Common Deviations**:
- Large field count (100+) → Takes longer but works fine
- Client variations (5+ sheets) → All documented
- Multiple segments (Header/Detail/Trailer) → Auto-segmented
- Unmapped fields → Will be flagged in output

---

## Advanced Usage

### Custom Configuration

You can pass additional options:

```
/generate-business-rules
vendor_name: AnthemWGS_Claims
...
custom_options:
  output_directory: /custom/delivery/path
  skip_validation: false
  include_sample_data: true
  client_variations: true
```

### Batch Processing with Status Monitoring

Process multiple vendors with feedback:

```
/generate-business-rules
batch_mode: true
vendors:
  - AnthemWGS_Claims
  - Navitus_Eligibility
  - BCBSAZJA_Claims
  - ArchimedesRX_Accums-Medical
  - MediKeeper_Claims

with_status_updates: true
  update_frequency: 1min
```

You'll get updates every minute on progress of each vendor.

---

## Integration with Your Workflow

### After Skill Completes

1. **Review the output**
   ```
   Open: /HRP Delivery/[DATE]/[VendorName]/Extract Criteria & Business Rules/Business_Rules_*.xlsx
   ```

2. **Check for unmapped fields**
   ```
   In Business Rules file, go to "Transformation Rules Summary" sheet
   Look for any fields listed as "Unmapped"
   Decide: Keep unmapped or manually add code location
   ```

3. **Validate a few examples**
   ```
   Pick 5-10 random fields from Business Rules
   Verify code locations are correct in actual C# code
   Spot-check transformation logic
   ```

4. **Commit to GitHub** (when satisfied)
   ```
   git add HRP Delivery/
   git commit -m "Add Business Rules for [VendorName] - generated by skill"
   git push
   ```

### Handling Output Quality Issues

If output isn't accurate:

1. **Check input paths** - Verify codebase paths are correct
2. **Verify layout file** - Ensure it's the right vendor layout
3. **Re-run skill** - With corrected inputs
4. **Manual review** - Check problem areas in detail
5. **Escalate** - If systematic issue, document and report

---

## Frequently Asked Questions

**Q: Can I process multiple vendors at once?**  
A: Yes! Use the `vendors:` array. Processing 3 vendors in parallel takes ~25 minutes instead of 30+ sequential.

**Q: What if the C# code for a vendor doesn't exist yet?**  
A: Skill will flag unmapped fields. You can still use the layout and business logic. Code location will be empty for unmapped fields.

**Q: How accurate is the output?**  
A: 100% for fields found in code (verified against actual source). Unmapped fields are clearly flagged. No hallucinated data.

**Q: Can I edit the Excel files after generation?**  
A: Yes! The files are just Excel. You can add notes, correct any issues, add examples. Keep the structure intact.

**Q: What if a vendor has client-specific variations?**  
A: Skill documents all client sheets in layout file. Main Business Rules file covers standard fields. Client variations noted.

**Q: How do I handle fields that are truly unmapped?**  
A: Document them in the final Excel file. Mark as "Unmapped - Not Implemented Yet" or "Unmapped - Handled Differently". Escalate to development team if needed.

---

**Guide Version**: 1.0.0  
**Last Updated**: July 28, 2026
