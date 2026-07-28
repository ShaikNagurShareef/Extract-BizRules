# Business Rules Generation Skill - Architecture Documentation

## System Overview

The `generate-business-rules` skill provides an automated, agent-based workflow for extracting accurate Business Rules documentation from vendor layouts and validating them against actual codebase implementations.

### Core Principle: No Hallucinations

Every field mapping, code location, and business rule **must be traced to actual source**:
- Layout fields from vendor mapping Excel files
- C# properties from actual domain model classes
- SQL procedures from actual stored procedures
- Transformation logic from actual code implementation

## System Architecture

```
┌─────────────────────────────────────────────────────┐
│         USER PROVIDES VENDOR CONFIGURATION           │
│  (vendor_name, layout_file, codebase paths, etc.)    │
└───────────────────────┬─────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────┐
│         INPUT VALIDATION & PRE-FLIGHT CHECKS        │
│  • Verify vendor folder exists                      │
│  • Confirm layout file accessible                   │
│  • Check C# extractor files present                 │
│  • Validate SQL procedures exist                    │
│  • Report back to user for confirmation             │
└───────────────────────┬─────────────────────────────┘
                        │
                        ▼ (User confirms "proceed")
┌─────────────────────────────────────────────────────┐
│      FIELD SEGMENTATION (if 100+ fields)            │
│  • Group by record type (Header/Detail/Trailer)     │
│  • Identify client variations                       │
│  • Create processing segments for accuracy          │
└───────────────────────┬─────────────────────────────┘
                        │
        ┌───────────────┼───────────────┬────────────┐
        │               │               │            │
        ▼               ▼               ▼            ▼
    ┌───────┐      ┌───────┐      ┌────────┐    ┌─────────┐
    │ AGENT │      │ AGENT │      │ AGENT  │    │ AGENT   │
    │   1   │      │   2   │      │   3    │    │    4    │
    │Layout │      │ Code  │      │  SQL   │    │Business │
    │Ana...  │      │Tracer │      │Validat│    │ Logic   │
    │       │      │       │      │       │    │         │
    └───┬───┘      └───┬───┘      └───┬───┘    └────┬────┘
        │              │             │             │
        │ Fields +     │ Code locs + │ SQL refs + │ Hard-coded
        │ positions    │ properties  │ transforms │ values
        │              │             │            │
        └──────────────┼─────────────┴────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
        │     AGGREGATION & VALIDATION  │
        │ • Merge results from agents   │
        │ • Verify field count match    │
        │ • Cross-reference locations   │
        │ • Flag unmapped fields        │
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
        │    README GENERATOR AGENT     │
        │ • Create Excel documentation │
        │ • Generate README files      │
        │ • Organize folder structure  │
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
        │   OUTPUT DELIVERY PACKAGING  │
        │ • Create /HRP Delivery folder│
        │ • Organize by date and vendor│
        │ • Create summary report      │
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
        │   READY FOR HRP IMPLEMENTATION│
        │ ✓ Business Rules files       │
        │ ✓ Validation reports         │
        │ ✓ README documentation       │
        └──────────────────────────────┘
```

## Agent Team Composition

### 1. Layout Analyzer Agent

**Purpose**: Extract field definitions from vendor layout files

**Inputs**:
- Vendor layout file path (.xlsx)
- Domain type (Claims, Eligibility, Accumulators, etc.)

**Process**:
1. Open vendor layout file
2. Identify all sheets (may include client-specific variations)
3. For each field, extract:
   - Field name
   - Position (start column, length if applicable)
   - Data type (Alphanumeric, Numeric, Date)
   - Length in characters
   - Required or Optional
   - Segment/record type (Header, Detail, Trailer)
4. Organize fields by segment for clarity

**Outputs**:
```json
{
  "vendor_name": "AnthemWGS_Claims",
  "total_fields": 145,
  "fields": [
    {
      "name": "ClaimID",
      "position": 1,
      "type": "A",
      "length": 20,
      "required": true,
      "segment": "Detail",
      "source_sheet": "Mapping"
    },
    ...
  ],
  "client_variations": [
    {
      "client": "ABA Emp Services",
      "variations": ["ClaimID format differs"]
    }
  ]
}
```

**Quality Metrics**:
- All fields extracted (100% coverage)
- Positions verified against source
- Data types correctly identified
- Client variations documented

### 2. Code Tracer Agent

**Purpose**: Find C# property mappings for layout fields

**Inputs**:
- Field list from Layout Analyzer
- C# extractor file path
- Vendor name

**Process**:
1. Search C# domain model for vendor-specific record class (e.g., AnthemWgsDetailRecord.cs)
2. For each field, find corresponding C# property:
   - Match by name (exact or similar)
   - Extract property type (string, int, DateTime, etc.)
   - Find property definition location (file, line number)
3. Identify transformation types:
   - **1-to-1**: Direct mapping (field → property)
   - **Derived**: Property computed from other properties
   - **Lookup**: Property value from crosswalk lookup
   - **Hard-Coded**: Property always set to fixed value
4. Extract code comments explaining business logic

**Outputs**:
```json
{
  "fields_with_code_matches": [
    {
      "layout_field": "ClaimID",
      "code_property": "ClaimID",
      "code_file": "AnthemWgsDetailRecord.cs",
      "code_line": 42,
      "property_type": "string",
      "transformation_type": "1-to-1",
      "business_logic": "Direct property assignment"
    },
    {
      "layout_field": "GroupNumber",
      "code_property": "GroupNumber",
      "code_file": "AnthemWgsDetailRecord.cs",
      "code_line": 87,
      "property_type": "string",
      "transformation_type": "Lookup",
      "business_logic": "Crosswalk lookup on Person.ParentPlan.PlanNum"
    }
  ],
  "unmapped_fields": [
    {
      "field": "MemberAltID",
      "reason": "Not found in code"
    }
  ]
}
```

**Quality Metrics**:
- Code locations exact (file + line number)
- Transformation types correctly identified
- Business logic extracted from code
- Unmapped fields clearly marked

### 3. SQL Validator Agent

**Purpose**: Extract SQL transformation logic

**Inputs**:
- Field list from Layout Analyzer
- Vendor name
- SQL stored procedure path

**Process**:
1. Search for vendor-specific stored procedures
2. For each field that uses SQL transformation:
   - Find relevant stored procedure
   - Extract transformation SQL (CASE statements, lookups, etc.)
   - Identify any lookups or crosswalks
   - Document parameter mappings
3. Identify data quality checks

**Outputs**:
```json
{
  "sql_procedures": [
    {
      "name": "sp_Transform_AnthemWGS_Claims",
      "file": "StoredProc_AnthemWGS_Claims.sql",
      "transformations": [
        {
          "field": "CertNumber",
          "sql": "CASE WHEN CertNumber IS NULL THEN 'UNKNOWN' ELSE CertNumber END",
          "line_number": 156
        }
      ]
    }
  ],
  "crosswalks": [
    {
      "lookup_name": "GroupNumber_Crosswalk",
      "source_field": "PlanNum",
      "target_field": "GroupNumber",
      "procedure": "sp_Lookup_GroupNumber"
    }
  ]
}
```

**Quality Metrics**:
- SQL procedures found and referenced
- Transformation logic extracted
- Crosswalks documented
- Data quality rules identified

### 4. Business Logic Agent

**Purpose**: Document hard-coded values, transformation rules, and business context

**Inputs**:
- Code analysis results from Code Tracer
- SQL results from SQL Validator
- Original layout specifications

**Process**:
1. Identify all hard-coded values in code
2. For each hard-coded value:
   - Extract the value
   - Find explanation in code comments
   - Determine if ALWAYS or CONDITIONAL
   - Document business reason (WHY)
3. Extract transformation logic patterns
4. Identify edge cases and special handling

**Outputs**:
```json
{
  "hard_coded_values": [
    {
      "property": "AdjustmentType",
      "value": "MED",
      "always_applied": true,
      "business_reason": "This vendor only processes medical claims",
      "code_location": "AnthemWgsDetailRecord.cs:95",
      "applied_when": "Always for all records"
    }
  ],
  "transformation_rules": [
    {
      "field": "Hcid",
      "rule": "Extract substring positions 3-14 from PolicyHolderAlternateKey",
      "business_reason": "HCID is embedded in CHID starting at position 3",
      "example": "CHI123456789ABC → 234567890AB"
    }
  ]
}
```

**Quality Metrics**:
- Hard-coded values with WHY documented
- Transformation rules clear and actionable
- Examples provided for each rule
- Business context explained

### 5. README Generator Agent

**Purpose**: Create comprehensive documentation for ALL artifacts

**Inputs**:
- All agent outputs (field mappings, code traces, SQL logic, validation results)
- Vendor configuration
- Package metadata

**Process**:
1. **Create Master Business Rules File** (Business_Rules_[VendorName].xlsx):
   - Executive Summary sheet
   - Transformation Rules Summary (HRP view)
   - Field mapping sheets (by segment type)
   - Hard-Coded Values sheet (with WHY documented)
   - Lookups & Crosswalks sheet
   - Code Locations sheet
   - SQL Transformations sheet
   - Validation Rules sheet

2. **Create Comprehensive README Documentation**:
   - DELIVERY_MANIFEST.md (master overview of entire package)
   - Extract Criteria & Business Rules/README.md (Excel structure guide)
   - Layout Verification/README.md (validation methodology & results)
   - Code_Tracing/README.md (how to read code locations)
   - SQL_Analysis/README.md (how SQL transformations work)
   - OriginalVendorLayouts/README.md (explains vendor mapping files)

3. **Create Detailed Analysis Documents**:
   - Field_Mapping_Guide.md (field extraction methodology)
   - Transformation_Rules.md (all transformation types explained)
   - Business_Logic_Documentation.md (hard-coded values & WHY)
   - Code_Locations.md (C# properties with line numbers)
   - SQL_Procedures.md (stored procedures with locations)
   - Transformation_Implementation.md (how code/SQL transforms data)
   - Crosswalks_and_Lookups.md (all lookups documented)
   - Client_Variations.md (if applicable - all client-specific changes)
   - Layout_Discovery_Report.md (sheet discovery & standardization)
   - Layout_Analysis.md (validation approach & methodology)
   - Validation_Summary.md (results, gaps, coverage statistics)

4. **Validate All Documentation**:
   - Ensure all files referenced actually exist
   - Verify code locations are accurate
   - Cross-check field counts and mappings
   - Validate examples are correct
   - Confirm README files are helpful and complete

**Outputs**:
- Complete Excel workbook with 8+ structured sheets
- 10+ comprehensive README files (one for each artifact type)
- Organized folder structure with clear documentation at each level
- Master DELIVERY_MANIFEST.md guiding through entire package

## Workflow Execution Model

### Single Vendor Workflow

```
Time  │ Input    │ Layout   │ Code     │ SQL      │ Business │ README   │ Output
      │ Validate │ Analyzer │ Tracer   │ Validator│ Logic    │ Generator│ Delivery
──────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼─────────
0 min │ ✓        │          │          │          │          │          │
      │ START    │ START    │ START    │ START    │ START    │          │
 1 min│          │ 20%      │ 15%      │ 10%      │ 20%      │          │
 2 min│          │ 40%      │ 35%      │ 25%      │ 40%      │          │
 3 min│          │ 60%      │ 55%      │ 50%      │ 60%      │          │
 4 min│          │ 80%      │ 75%      │ 70%      │ 80%      │          │
 5 min│          │ ✓ DONE   │ 90%      │ 85%      │ ✓ DONE   │          │
 6 min│          │          │ ✓ DONE   │ 95%      │          │          │
 7 min│          │          │          │ ✓ DONE   │          │ START    │
 8 min│          │          │          │          │          │ 50%      │
10 min│          │          │          │          │          │ ✓ DONE   │ ✓ READY
```

**Total Time**: 8-12 minutes for typical vendor (50-150 fields)

### Multiple Vendor Parallel Execution

```
Vendor 1:  V1-Start → V1-Agents (1-7min) → V1-README (8-10min) → V1-DONE
Vendor 2:                V2-Start → V2-Agents (1-7min) → V2-README (8-10min) → V2-DONE
Vendor 3:                           V3-Start → V3-Agents (1-7min) → V3-README (8-10min) → V3-DONE

Total Time for 3 vendors in parallel: ~25 minutes (vs ~30 sequential)
```

## Data Structures

### Field Definition Schema

```json
{
  "layout_field_name": "string",
  "position": "number or object with start/length",
  "data_type": "A | N | D | AN", // Alphanumeric, Numeric, Date, AlphaNumeric
  "length": "number",
  "required": "boolean",
  "segment_type": "Header | Detail | Trailer",
  "code_property": "string or null",
  "code_file": "string or null",
  "code_line": "number or null",
  "transformation_type": "1-to-1 | Derived | Lookup | Hard-Coded | Unmapped",
  "business_rule": "string",
  "validation_rule": "string or null",
  "examples": [
    {
      "layout_input": "string",
      "code_output": "string",
      "explanation": "string"
    }
  ]
}
```

### Business Rules Sheet Schema

Excel sheet columns:
1. **Sequence** - Field order (1, 2, 3, ...)
2. **Layout Field Name** - From vendor layout
3. **Position** - Column position or positions
4. **Type** - A/N/D/AN
5. **Length** - Character length
6. **Required** - Yes/No
7. **Code Property** - C# property name
8. **Code File** - Which .cs file
9. **Code Line** - Line number in file
10. **Transformation Type** - How it's transformed
11. **Business Rule** - WHY it's transformed
12. **SQL Logic** - Any SQL transformations
13. **Hard-Coded Value** - If value is fixed
14. **Validation Rule** - How to validate
15. **Example** - Sample transformation
16. **Notes** - Additional context

## Validation Strategy

### Field Count Validation
- Count fields in layout file
- Count fields found in code
- Flag if significantly different
- Investigate discrepancies

### Code Location Verification
- Verify each C# file exists
- Verify line numbers point to property definitions
- Verify method names match expected pattern

### Transformation Logic Verification
- Confirm transformation types match code behavior
- Verify SQL procedures exist if referenced
- Validate lookup table references

### Output Quality Checks
- All sheets present in Excel file
- All required columns populated
- No placeholder or generic content
- README files clear and complete

## Error Handling Strategy

### Missing Code
**Agent**: Code Tracer  
**Detection**: Field not found in C# code  
**Action**: Mark as unmapped, continue processing  
**User Notification**: List unmapped fields in final report

### Missing SQL
**Agent**: SQL Validator  
**Detection**: Stored procedure not found  
**Action**: Note absence, continue with other validations  
**User Notification**: Flag in transformation rules sheet

### Mismatched Counts
**Agent**: Aggregation step  
**Detection**: Layout field count ≠ code fields found  
**Action**: List specific differences  
**User Notification**: Ask for clarification or manual mapping

### File Access Issues
**Agent**: Any agent  
**Detection**: Cannot read file or access path  
**Action**: Stop immediately, report issue  
**User Notification**: Provide exact path that failed and suggested fix

## Extensibility

### Adding New Agents

To add a new agent to the workflow:

1. **Define agent purpose** - What specific task does it perform?
2. **Design inputs** - What data does it need from other agents?
3. **Design outputs** - What structure should it produce?
4. **Implement extraction logic** - Code to find and extract data
5. **Add validation** - Verify output is complete and accurate
6. **Integrate into workflow** - Add to orchestration, set dependencies
7. **Update documentation** - Explain new agent in architecture docs

### Adding New Vendors

To support a new vendor:

1. **Identify extractor location** - Where is C# code for this vendor?
2. **Identify record class** - What class implements the detail record?
3. **Add vendor mapping** - Map vendor name to code path
4. **Test with sample** - Run skill with test vendor
5. **Validate accuracy** - Spot-check against actual code

## Performance Characteristics

| Component | Time | Notes |
|-----------|------|-------|
| Input Validation | <1 min | Quick file checks |
| Layout Analysis | 2-5 min | Depends on field count |
| Code Tracing | 3-6 min | Searching C# code |
| SQL Validation | 2-4 min | Finding procedures |
| Business Logic | 2-4 min | Extracting rules |
| README Generation | 1-2 min | Creating documentation |
| **Total** | **8-12 min** | Per vendor (50-150 fields) |

For 200+ field vendors:
- Layout Analysis: 5-8 min
- Code Tracing: 6-10 min
- SQL Validation: 4-6 min
- Business Logic: 4-6 min
- README Generation: 2-3 min
- **Total**: **15-25 min**

Parallel execution of 3 vendors: ~25 minutes (not 45)

## Integration Points

### GitHub Integration
- Skill code committed to GitHub
- Version tags for releases
- Collaboration via pull requests

### Claude Code Integration
- Skill invoked via `/generate-business-rules` command
- Runs in Claude Code session
- Produces local files ready for git commit

### HRP System Integration
- Output formatted for HRP's requirements
- README files explain HRP implementation
- Files organized for immediate deployment

---

**Architecture Version**: 1.0.0  
**Last Updated**: July 28, 2026  
**Documented By**: Claude Code + HRP Team
