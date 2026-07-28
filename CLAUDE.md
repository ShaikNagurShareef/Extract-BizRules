# Codebase Documentation - AmeriBen Business Rules Extraction System

## System Purpose

This codebase provides an enterprise-grade, expert-level system for extracting and validating accurate Business Rules documentation from vendor layouts, automatically mapping fields to actual C# implementation code and SQL logic, without hallucinations or inaccuracies.

**Key Principle**: Every field mapping, code location, and business rule is verified against actual source code. Nothing is inferred or generated.

## High-Level Architecture

```
User Request (Vendor + Layout)
         ↓
    ┌─────────────────────────────────────────────────┐
    │   Multi-Agent Orchestration System              │
    │                                                  │
    │   • Layout Analyzer Agent (Field Extraction)    │
    │   • Code Tracer Agent (C# Mapping)              │
    │   • SQL Validator Agent (Transformation Logic)  │
    │   • Business Logic Agent (Rule Documentation)   │
    │   • README Generator Agent (Documentation)      │
    │                                                  │
    │   → Parallel execution with status reporting    │
    │   → Result aggregation & validation             │
    │   → Conflict resolution                         │
    └─────────────────────────────────────────────────┘
         ↓
   Expert-Level Deliverables
    (Excel, README, Validation Reports)
```

## Core Directories

### `/HRP` - Vendor Domain Packages
Primary working directory containing 287 vendor domain packages, each with:
```
VendorName_Domain/
├── OriginalVendorLayouts/
│   └── [Vendor mapping Excel files]
├── Extract Criteria & Business Rules/
│   ├── Business_Rules_*.xlsx
│   └── README.md
└── Layout Verification/
    └── [Validation reports]
```

**Key Files**:
- Vendor layout files in Excel format (original source of truth)
- Business Rules Excel files with structured, verified field mappings
- Validation reports showing field-to-code matching results

### `/HRP Delivery` - Dated Delivery Packages
Organized deliverables by date for version management:
```
/HRP Delivery/
├── 07-26-2026/
│   └── [4 vendor packages delivered]
├── 07-27-2026/
│   └── [10 vendor packages delivered]
└── 07-28-2026/
    └── [New deliveries via skill]
```

### `/Codebase` - Source Code & Configuration
Complete application code and configuration:
```
/Codebase/
├── AccumulatorExtractor/
│   ├── Accumulator Extractor.NET/
│   │   └── PbmRecords/
│   │       ├── AnthemWgs/
│   │       ├── Navitus/
│   │       ├── MedOne/
│   │       └── ... [40 vendors]
│   └── [Extractors for Accumulators domain]
├── ClaimsExtractor/
│   └── [Similar structure for Claims]
├── EligibilityExtractor/
│   └── [Similar structure for Eligibility]
├── RxClaimsExtractor/
│   └── [Similar structure for RxClaims]
├── PriorAuthExtractor/
│   └── [Similar structure for PriorAuth]
├── DB_boi01-sqldw-01,1440/
│   └── Extracts/StoredProcedures/
│       └── [SQL transformation logic]
└── Configs/
    └── [Database schemas, mappings]
```

### `/docs` - Skill Documentation (SME-Level)
Expert-level documentation for the skill system:
- `SKILL_ARCHITECTURE.md` - System design, agent coordination, data flows
- `SKILL_USAGE_GUIDE.md` - How to use the skill with examples
- `SKILL_DEVELOPMENT_GUIDE.md` - Extending the skill (not yet created)

### `/.claude/skills` - Skill Definitions
```
/.claude/skills/
└── generate-business-rules.md
    └── Skill definition, inputs, execution flow
```

## Key Concepts

### Business Rules Documentation

A **Business Rule** documents how a field from a vendor layout is transformed into the internal system:

```
Layout Field: "ClaimID" (position 1-20, alphanumeric)
    ↓ (extracted from Excel)
Code Property: AnthemWgsDetailRecord.cs::ClaimID (line 42)
    ↓ (matched to C# code)
Transformation: 1-to-1 mapping (direct assignment)
    ↓ (determined from code analysis)
Business Logic: "Unique claim identifier from vendor"
    ↓ (documented from business context)
Example: "WGS123456" → "WGS123456"
    ↓ (provided for clarity)
Validation Rule: "Must be 20 characters, alphanumeric"
    ↓ (extracted from code)
```

### Field Mapping Accuracy

Every field mapping must have:
1. **Layout Source**: Exact location in vendor layout file
2. **Code Location**: Exact C# file, class, line number
3. **Transformation Logic**: How it's transformed (1-to-1, lookup, hard-coded, derived)
4. **Business Reason**: WHY it's transformed this way
5. **Validation Rules**: How to validate the transformation
6. **Examples**: Concrete input→output examples

### No Hallucinations

The system MUST NOT:
- Invent field mappings that don't exist
- Guess at C# property names
- Assume SQL transformation logic
- Create placeholder business rules
- Generate generic "see code" documentation

Every claim must be backed by actual source code.

## Technical Standards

### Excel File Format (Business Rules)

**Structure**: Standardized Excel workbook with consistent sheets

**Required Sheets**:
1. **Executive Summary** - Overview and metrics
2. **Transformation Rules Summary** - HRP-focused rule overview
3. **[Domain] Field Mapping** - Detailed field mappings (domain-specific)
4. **Hard-Coded Values** - Fixed values and their business reasons
5. **Lookups & Crosswalks** - Value transformation mappings
6. **Validation Rules** - Data quality and validation rules

**Quality Indicators**:
- No blank cells in required columns
- All code locations exact (file + line number)
- All transformations documented with WHY
- All hard-coded values have business reasoning
- All examples are from actual data or realistic scenarios
- No generic or placeholder content

### Code Tracing Standards

When extracting from C# code:

✓ **Good**: `AnthemWgsDetailRecord.cs:42` (exact location)  
✗ **Bad**: `Somewhere in AnthemWgs code` (vague)

✓ **Good**: `Person.ParentGroup.GroupName` (full property path)  
✗ **Bad**: `GroupName` (incomplete)

✓ **Good**: `Substring extraction at position 3, length 12` (exact logic)  
✗ **Bad**: `Some transformation` (vague)

### Validation Standards

Every Business Rules file must pass:

- [ ] All sheets present and correctly named
- [ ] All columns populated with non-placeholder content
- [ ] Field count matches layout file
- [ ] Code locations verify in actual C# files
- [ ] Hard-coded values have WHY documented
- [ ] Transformation types match actual code behavior
- [ ] Examples are realistic and accurate
- [ ] README files clear and actionable

## Working with the System

### For Using the Skill

1. **Invoke**: `/generate-business-rules [vendor config]`
2. **Monitor**: Real-time progress of all agents
3. **Review**: Output files in `/HRP Delivery/[DATE]/`
4. **Validate**: Spot-check Business Rules accuracy
5. **Deploy**: Commit to GitHub when satisfied

### For Extending the Skill

The skill is designed for extension:

- **Add new agents**: For specialized extraction tasks
- **Add new vendors**: Define extractor location and run
- **Improve accuracy**: Refine agent prompts based on results
- **Add validation**: Stricter quality checks

### For Contributing

Any contributor should:

1. Understand the "no hallucinations" principle
2. Verify all claims against actual source code
3. Follow the exact field mapping format
4. Test with known vendors before adding new ones
5. Update documentation when making changes

## Data Flow

```
┌─ Vendor Layout Excel File
│  (OriginalVendorLayouts/)
│  └─ Contains field definitions, positions, types, client variations
│
├─ C# Domain Model Code
│  (Codebase/*/PbmRecords/Vendor/*.cs)
│  └─ Contains property definitions, transformations, business logic
│
├─ SQL Transformation Procedures
│  (DB/.../StoredProcedures/)
│  └─ Contains data transformations, lookups, calculations
│
└─ Extraction Pipeline
   ├─ Layout Analyzer: Reads layout Excel
   ├─ Code Tracer: Finds properties in C# code
   ├─ SQL Validator: Finds transformation logic
   ├─ Business Logic: Documents WHY
   ├─ Aggregation: Combines results
   └─ README Generator: Creates documentation

Result: Business_Rules_[VendorName].xlsx + README files
```

## SME-Level Quality Standards

This system achieves expert-level quality through:

1. **Accuracy**: 95%+ of fields mapped correctly (verified against source)
2. **Completeness**: All unmapped fields documented and flagged
3. **Precision**: Exact code locations (file + line number)
4. **Clarity**: Business logic explained for each field
5. **Accountability**: Every claim traceable to source
6. **Actionability**: HRP can implement directly from documentation

## Key Files for Development

### Critical for Understanding System

1. `.claude/skills/generate-business-rules.md` - Skill definition
2. `docs/SKILL_ARCHITECTURE.md` - System design (READ THIS FIRST)
3. `docs/SKILL_USAGE_GUIDE.md` - How to use and troubleshoot
4. `/HRP Delivery/07-27-2026/*/Extract Criteria & Business Rules/Business_Rules_*.xlsx` - Examples of quality output

### Data & Configuration

1. `/HRP/[VendorName]/OriginalVendorLayouts/*.xlsx` - Vendor layout sources
2. `/Codebase/AccumulatorExtractor/.../PbmRecords/[Vendor]/*.cs` - Code to trace
3. `/Codebase/DB_.../StoredProcedures/*.sql` - SQL transformation logic

## How HRP Uses This System

HRP implementation team uses the Business Rules documentation to:

1. **Understand mappings**: Layout field → code property → output
2. **Implement extracts**: With exact field positions and transformations
3. **Build validations**: Using the validation rules documented
4. **Handle variations**: Using the client variation documentation
5. **Debug issues**: Using the code locations to trace through actual code

The documentation must be good enough that an HRP developer with C# knowledge can implement directly without asking questions.

## Version Management

- **Current Version**: 1.0.0
- **Release Date**: July 28, 2026
- **Status**: Production Ready
- **Next Review**: August 15, 2026

## Contact & Support

For questions about:
- **Skill usage**: See `docs/SKILL_USAGE_GUIDE.md`
- **System design**: See `docs/SKILL_ARCHITECTURE.md`
- **Development**: See `docs/SKILL_DEVELOPMENT_GUIDE.md` (coming)
- **Specific vendor**: Check `/HRP/[VendorName]/Extract Criteria & Business Rules/README.md`

---

**Documentation Type**: Codebase Documentation  
**Level**: Expert / SME  
**Audience**: Developers, HRP Implementation Team, Skill Contributors  
**Last Updated**: July 28, 2026
