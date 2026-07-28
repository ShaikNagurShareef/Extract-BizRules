# Skill: generate-business-rules

Extract and validate Business Rules from vendor layouts against codebase, generating accurate documentation with field mappings, transformations, and comprehensive README files.

## What This Skill Does

Orchestrates an automated workflow to generate HRP-compliant Business Rules documentation for vendor packages:

1. **Validates** vendor layout files against actual C# code and SQL procedures
2. **Extracts** field mappings with exact code locations (no hallucinations)
3. **Documents** transformation rules, hard-coded values, and business logic
4. **Creates** standardized Business Rules Excel files with HRP-compliant structure
5. **Generates** comprehensive README files explaining all artifacts
6. **Organizes** complete deliverable packages for immediate HRP implementation
7. **Supports** parallel processing of multiple vendor packages

## REQUIRED Inputs

You **MUST** provide these inputs, or the skill cannot proceed:

### 1. Vendor Configuration (Required)
- **vendor_name**: Exact vendor folder name (e.g., "AnthemWGS_Claims", "Navitus_Eligibility")
- **domain_type**: One of: Claims, Eligibility, Accumulators, RxClaims, PriorAuth, FinancialReconciliation
- **layout_file_path**: Full path to vendor layout file (e.g., `/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx`)

### 2. Codebase Paths (Required)
- **codebase_root**: Root path to source code (e.g., `{PROJECT_ROOT}/Codebase/`)
- **c_sharp_extractor_path**: Relative path to C# extractor files (e.g., `AccumulatorExtractor/Accumulator Extractor.NET/.../*.cs`)
- **sql_root_path**: Relative path to SQL procedures (e.g., `DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/`)

### 3. Business Context (Optional but Recommended)
- **business_rules_doc**: Path to existing business documentation (optional)
- **vendor_notes**: Text describing vendor-specific quirks (optional)
- **client_variations**: Does this vendor have client-specific variations? (Y/N, optional)

## How to Use This Skill

### Step 1: Prepare Your Inputs
Gather all required information:
```
vendor_name = "AnthemWGS_Claims"
domain_type = "Claims"
layout_file_path = "{PROJECT_ROOT}/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx"
codebase_root = "{PROJECT_ROOT}/Codebase/"
c_sharp_extractor_path = "AccumulatorExtractor/Accumulator Extractor.NET/AccumulatorExtractor.NET/PbmRecords/AnthemWgs/*.cs"
sql_root_path = "DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/"
```

### Step 2: Invoke the Skill
Use the skill with your vendor configuration. The skill will:
- Validate all inputs exist and are accessible
- Display pre-flight check report
- Request your confirmation to proceed
- Execute workflow if all checks pass

### Step 3: Monitor Progress
The skill will display real-time progress:
```
[PARALLEL AGENT EXECUTION]
🔄 Layout Analyzer: 45% complete (3 min remaining)
🔄 Code Tracer: 60% complete (2 min remaining)
🔄 SQL Validator: 40% complete (4 min remaining)
...
```

### Step 4: Review Output
The skill creates:
- `Business_Rules_[VendorName].xlsx` - Complete Business Rules documentation
- `Layout Verification/` - Validation reports
- `README.md` files - Explaining all artifacts

## Pre-Flight Checks

Before the skill starts, it will validate:

✓ Vendor folder exists in HRP  
✓ Layout file exists and is readable  
✓ C# extractor files exist  
✓ SQL stored procedures exist  
✓ Domain type is recognized  
✓ All paths are accessible

If ANY check fails, you'll be notified IMMEDIATELY with guidance on how to fix it.

## Example Invocation

```
/generate-business-rules
vendor_name: AnthemWGS_Claims
domain_type: Claims
layout_file_path: {PROJECT_ROOT}/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: AccumulatorExtractor/Accumulator Extractor.NET/AccumulatorExtractor.NET/PbmRecords/AnthemWgs/*.cs
sql_root_path: DB_boi01-sqldw-01,1440/Extracts/StoredProcedures/
```

## Processing Multiple Vendors

Process multiple vendors in parallel for faster delivery:

```
/generate-business-rules
vendors:
  - vendor_name: AnthemWGS_Claims
    layout_file_path: ...
  - vendor_name: Navitus_Eligibility
    layout_file_path: ...
  - vendor_name: BCBSAZJA_Accums-Medical
    layout_file_path: ...
codebase_root: {PROJECT_ROOT}/Codebase/
```

Estimated time for 3 vendors: ~25 minutes (parallel execution)

## Deviation Handling

If the skill detects any deviations, it will **IMMEDIATELY** notify you:

```
⚠️ DEVIATION DETECTED:
   Issue: Layout file has 200+ fields (more than typical)
   Impact: Processing may take 15-20 minutes
   Action: Skill will segment by record type for accuracy
   User Action: None required - skill handles automatically
```

## Error Handling

If an agent encounters an error:

```
❌ ERROR IN CODE TRACER AGENT:
   Vendor field 'MemberID' not found in C# model
   Searched: /Codebase/ClaimsExtractor/PbmRecords/AnthemWGS/*.cs
   
   RESOLUTION OPTIONS:
   1. Verify field name in code (provide correct name)
   2. Mark as unmapped and continue
   3. Provide custom code location manually
   
   What should I do? (1/2/3)
```

## Output Structure

### What Gets Created

For each vendor, the skill creates **complete artifact packages with comprehensive README documentation**:

```
/HRP Delivery/[DATE]/[VendorDomain]/
├── OriginalVendorLayouts/
│   ├── [vendor mapping files - unchanged]
│   └── README.md (explains what files are here, how to read them)
│
├── Extract Criteria & Business Rules/
│   ├── Business_Rules_[VendorName].xlsx
│   ├── [Supporting _Business_Logic.xlsx files]
│   ├── Field_Mapping_Guide.md (documents field extraction methodology)
│   ├── Transformation_Rules.md (explains all transformation types)
│   ├── Business_Logic_Documentation.md (hard-coded values & WHY)
│   └── README.md (how to use the Business Rules files)
│
├── Layout Verification/
│   ├── Validation_Report_[VendorName].xlsx
│   ├── Layout_Analysis.md (what was validated and how)
│   ├── Validation_Summary.md (results, discrepancies, coverage)
│   ├── Client_Variations.md (if applicable - documents all client-specific changes)
│   ├── Layout_Discovery_Report.md (sheet discovery, classification, standardization)
│   └── README.md (how to interpret validation reports)
│
├── Code_Tracing/
│   ├── Code_Locations.md (all C# properties mapped with exact line numbers)
│   ├── Transformation_Implementation.md (how each field is transformed in code)
│   └── README.md (guide to understanding code locations)
│
├── SQL_Analysis/
│   ├── SQL_Procedures.md (stored procedures used, with locations)
│   ├── Transformation_Logic.md (SQL transformations documented)
│   ├── Crosswalks_and_Lookups.md (all lookup tables and crosswalks)
│   └── README.md (how SQL transformations work)
│
└── DELIVERY_MANIFEST.md (master document explaining entire package)
```

**CRITICAL**: Every artifact comes with a README file explaining:
- What the file contains
- How to use it
- Key findings and important notes
- Where to find related information
- How it connects to other artifacts

### Business Rules File Structure

**Business_Rules_[VendorName].xlsx** includes:
- **Executive Summary** - Package overview and metrics
- **Transformation Rules Summary** - Rule-centric view for HRP
- **[Domain] Field Mapping** - Detailed field mappings (Claims, Eligibility, etc.)
- **Hard-Coded Values** - Hard-coded values with WHY documented
- **Lookups & Crosswalks** - Value transformation mappings
- **Validation Rules** - Field validation criteria

### README Files

**Extract Criteria & Business Rules/README.md**:
- Explains Business Rules structure
- Shows how to use each sheet
- Documents hard-coded values
- Lists all fields extracted

**Layout Verification/README.md**:
- Explains validation approach
- Shows field-to-code mapping results
- Documents any discrepancies found
- Provides next steps

## Expected Timeline

| Vendor Size | Fields | Time Estimate |
|-------------|--------|---------------|
| Small | <50 | 5-8 minutes |
| Medium | 50-150 | 10-15 minutes |
| Large | 150+ | 15-25 minutes |
| Multiple (3 in parallel) | - | ~25 minutes |

## Success Indicators

The skill completed successfully if:

✓ All agents completed without errors  
✓ Business Rules file created with all required sheets  
✓ Field count matches layout file  
✓ Code locations found for 95%+ of fields  
✓ README files clear and accurate  
✓ Files organized in proper delivery structure  

## Quality Metrics

The skill tracks and reports:
- **Field Coverage**: % of layout fields found in code
- **Unmapped Fields**: Fields not found in code (flagged)
- **Code Accuracy**: % of code locations verified
- **Hard-Coded Values**: Count and documentation completeness
- **Business Logic**: Transformation rules documented
- **Documentation Quality**: README clarity score

## Troubleshooting

### "Layout file not found"
**Fix**: Verify the exact file path exists. Check spelling and use absolute paths.

### "C# extractor files not found"
**Fix**: Verify codebase_root path is correct and contains AccumulatorExtractor or ClaimsExtractor folders.

### "Fields in layout don't match code"
**Fix**: Some vendors have different field naming. Skill will flag these as unmapped and document them.

### "Process takes longer than expected"
**Fix**: Large vendors (200+ fields) may take 20+ minutes. Skill will segment for accuracy.

## Known Limitations

- Skill requires C# code and layout files to exist
- SQL procedures must be in standard locations
- Does not create new extractors (only documents existing ones)
- Some vendor-specific transformations may need manual review
- Client variations require manual identification currently

## Questions or Issues?

- Check the **SKILL_USAGE_GUIDE.md** for detailed examples
- Review **SKILL_ARCHITECTURE.md** for system design details
- See **SKILL_DEVELOPMENT_GUIDE.md** for extending the skill

---

**Skill Version**: 1.0.0  
**Last Updated**: July 28, 2026  
**Maintainer**: HRP Team  
**Status**: Ready for Production
