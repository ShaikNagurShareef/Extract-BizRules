# API Reference - Extract-BizRules Skill

**Version**: 1.0.0 | **Complete API Documentation**

---

## Skill Invocation

### Primary Endpoint: `/generate-business-rules`

**Parameters:**

```
vendor_name (string, required)
  Exact vendor folder name
  Example: "AnthemWGS_Claims"

domain_type (string, required)  
  One of: Claims | Eligibility | Accumulators | RxClaims | PriorAuth | FinancialReconciliation
  
layout_file_path (string, required)
  Full path to vendor layout Excel file
  Example: "{PROJECT_ROOT}/HRP/[Vendor]/OriginalVendorLayouts/[file].xlsx"

codebase_root (string, required)
  Root path to C# source code
  Example: "{PROJECT_ROOT}/Codebase/"

c_sharp_extractor_path (string, required)
  Relative path to C# extractor files with wildcard
  Example: "AccumulatorExtractor/.../PbmRecords/[Vendor]/*.cs"

sql_root_path (string, required)
  Relative path to SQL procedures directory
  Example: "DB_.../Extracts/StoredProcedures/"

workflow (string, optional)
  Default: "complete-business-rules"
  Options:
    - complete-business-rules: Full documentation
    - field-specs-only: Quick field extraction
    - code-mapping-only: Field-to-code mapping
    - transformations-only: Transformation logic
    - specification-generation: Formal specs

batch_mode (boolean, optional)
  Enable parallel processing of multiple vendors
  Default: false

vendors (array, optional)
  When using batch mode, array of vendor configs
  Each item has: vendor_name, layout_file_path, domain_type

output_directory (string, optional)
  Where to save results
  Default: "{PROJECT_ROOT}/HRP Delivery/[DATE]/"

include_hard_coded_values (boolean, optional)
  Include hard-coded values documentation
  Default: true

output_format (string, optional)
  Default: "xlsx"
  Options: xlsx, csv, markdown, json
```

**Response:**

```
{
  status: "success" | "error" | "partial",
  message: "Detailed status message",
  deliverables: {
    business_rules_file: "path/to/Business_Rules_*.xlsx",
    validation_report: "path/to/Validation_Report_*.xlsx",
    readme_files: ["path/to/README_1.md", ...],
    layout_verification: "path/to/Layout_Verification/"
  },
  metrics: {
    total_fields: 145,
    mapped_fields: 141,
    unmapped_fields: 4,
    coverage_percentage: 97,
    processing_time_seconds: 720
  }
}
```

---

## Secondary Endpoint: `/q-and-a` (Interactive)

Ask questions about vendor implementation

**Parameters:**

```
vendor_name (string, required)
  Which vendor to query

codebase_root (string, required)
  Path to source code

layout_file_path (string, required)
  Path to layout file

sql_root_path (string, required)
  Path to SQL procedures

question (string or array, required)
  Single question or array of questions
  Examples:
    - "How is GroupNumber calculated?"
    - "Which fields are hard-coded?"
    - ["Q1", "Q2", "Q3"]
```

**Response:**

```
{
  status: "success",
  questions_answered: 3,
  answers: [
    {
      question: "...",
      answer: "...",
      code_references: [...],
      examples: [...]
    },
    ...
  ]
}
```

---

## Output File Formats

### Business_Rules_[Vendor].xlsx

**Sheets:**
1. Executive Summary
2. Transformation Rules Summary
3. [Domain] Field Mapping (Claims, Eligibility, etc.)
4. Hard-Coded Values
5. Lookups & Crosswalks
6. Validation Rules

**Columns in Field Mapping:**
- Sequence
- Layout Field Name
- Position
- Type
- Length
- Required
- Code Property
- Code File
- Code Line
- Transformation Type
- Business Rule
- SQL Logic
- Hard-Coded Value
- Validation Rule
- Example
- Notes

---

## Quality Metrics Returned

```
coverage: {
  total_fields: number,
  mapped_fields: number,
  unmapped_fields: number,
  coverage_percentage: 0-100
}

accuracy: {
  code_locations_verified: 0-100,
  business_rules_documented: 0-100,
  hard_coded_values_explained: 0-100
}

performance: {
  total_time_seconds: number,
  agents_execution_time: {
    layout_analyzer: number,
    code_tracer: number,
    sql_validator: number,
    business_logic: number,
    readme_generator: number
  }
}
```

---

**See USER_GUIDE.md for usage examples**
