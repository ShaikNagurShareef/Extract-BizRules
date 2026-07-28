# Comprehensive Field Tracing - Complete Journey Grounding

**Version**: 2.0.0 | **Complete End-to-End Field Logic**

---

## What This Means

Every field in every vendor layout will be traced through **EVERY location** where it appears in your system:

```
Layout Field
    ↓
[1] LAYOUT VALIDATION → Extract from Excel file
    ↓
[2] C# IMPLEMENTATION → Find property & transformations
    ↓
[3] BUSINESS LOGIC → Hard-coded values, rules, conditions
    ↓
[4] SQL PROCEDURES → Data transformations & lookups
    ↓
[5] DATABASE SCHEMA → Where field lands in DB tables
    ↓
[6] DEPENDENCY CHAIN → What other fields it depends on
    ↓
[7] OUTPUT TRANSFORMATION → How field is formatted for output
    ↓
[8] VALIDATION RULES → All validation across all locations
    ↓
Complete Field Documentation
```

---

## Enhanced Skill Architecture

### 8 Specialized Agents (vs current 5)

**AGENT 1: Layout Validation Extractor**
- Extract validation rules from Excel layout files
- Data validation rules, dropdowns, ranges, lists
- Field format constraints
- Required/optional indicators
- Length restrictions
- Allowable value lists
- Cross-field dependencies
- Notes and comments about validation

**Output**: Layout Validation Map

---

**AGENT 2: Code Implementation Tracer** (Enhanced)
- Find C# properties mapping to layout fields
- Extract transformation logic (1-to-1, lookup, derived, hard-coded)
- Document all code locations (file + line)
- Identify derived fields and their sources

**Output**: Code Property Mapping

---

**AGENT 3: Database Schema Mapper** (NEW)
- Trace where each field lands in database tables
- Find table relationships
- Identify column data types
- Find database constraints
- Document any data transformations at DB level
- Map to SQL procedures

**Output**: Database Schema Mapping

---

**AGENT 4: SQL Transformation Analyzer** (Enhanced)
- Extract ALL SQL transformation logic
- Stored procedures using the field
- Views that reference the field
- Calculations and aggregations
- Lookups and crosswalks
- Data quality checks at SQL level

**Output**: SQL Transformation Reference

---

**AGENT 5: Output Format Analyzer** (NEW)
- How fields are formatted for output files
- Output file position and format
- Field formatting rules (padding, truncation, etc.)
- Conditional formatting logic
- Output file structures
- Field order in output

**Output**: Output Formatting Rules

---

**AGENT 6: Dependency Chain Mapper** (NEW)
- Which fields this field depends on
- Which fields depend on this field
- Order of transformation dependencies
- Conditional dependencies
- Circular dependency detection
- Impact analysis

**Output**: Dependency Chain Map

---

**AGENT 7: Validation Rules Consolidator** (NEW)
- Collect ALL validation rules for field
  - From layout file (Excel validation)
  - From C# code (property validation)
  - From SQL procedures (DB constraints)
  - From output formatting (output rules)
- Cross-reference validations
- Identify gaps or conflicts
- Document validation at each level

**Output**: Complete Validation Map

---

**AGENT 8: README Generator** (Enhanced)
- Create comprehensive documentation
- Explain complete field journey
- Document all validation rules
- Show dependency chains
- Provide examples at each transformation step

**Output**: Complete documentation

---

## Output Structure - Enhanced Business Rules File

### Sheets in Business_Rules_[Vendor].xlsx

1. **Executive Summary** - Package overview & metrics
2. **Transformation Rules Summary** - Quick reference view
3. **[Domain] Field Mapping** - Field definitions (original)
4. **Layout Validation Rules** (NEW)
   - Validation rules from vendor layout file
   - Format constraints
   - Required/optional indicators
   - Allowable values
5. **C# Implementation** (Enhanced)
   - Property mappings
   - Transformation types
   - Code locations
   - Business logic
6. **Database Schema** (NEW)
   - Table names
   - Column names
   - Data types
   - Constraints
7. **SQL Transformations** (NEW)
   - Stored procedures
   - SQL logic
   - Lookups
   - Data quality checks
8. **Output Formatting** (NEW)
   - Output file format
   - Field position in output
   - Formatting rules
   - Examples
9. **Dependency Chain** (NEW)
   - Field dependencies
   - Transformation order
   - Impact analysis
10. **Complete Validation Rules** (NEW)
    - All validation at all levels
    - Cross-references
    - Gaps identified
    - Conflicts highlighted
11. **Hard-Coded Values** - Fixed values with reasoning
12. **Lookups & Crosswalks** - Value transformation mappings

---

## Example: Complete Field Tracing

### Field: GroupNumber

```
LAYOUT VALIDATION (from Excel):
  • Required: Yes
  • Type: Alphanumeric
  • Length: 10
  • Allowable Values: [dropdown list of valid group numbers]
  • Validation: Must be in GroupNumber_Crosswalk lookup
  ↓

C# IMPLEMENTATION:
  • Property: GroupNumber
  • File: AnthemWgsDetailRecord.cs:87
  • Transformation: Crosswalk lookup
  • Logic: Person.ParentPlan.PlanNum → GroupNumber crosswalk
  • Hard-Coded: No
  ↓

DATABASE SCHEMA:
  • Table: WGS_Claims_Detail
  • Column: GroupNumber
  • Data Type: VARCHAR(10)
  • Constraint: Foreign Key to GroupNumber_Lookup table
  ↓

SQL TRANSFORMATIONS:
  • Procedure: sp_Transform_AnthemWGS_Claims (line 156)
  • Logic: CASE WHEN GroupNumber IS NULL THEN 'UNKNOWN' ELSE GroupNumber END
  • Lookup: sp_Lookup_GroupNumber (line 189)
  ↓

OUTPUT FORMATTING:
  • Output File: Claims_Extract_*.txt
  • Position: Columns 21-30
  • Format: Left-aligned, padded with spaces
  • Example: "PPOLH     " (10 chars)
  ↓

DEPENDENCY CHAIN:
  • Depends On: Person.ParentPlan.PlanNum (required)
  • Used By: Claims Header (GroupNumber field)
  • Impact: If missing, claims cannot be processed
  ↓

VALIDATION RULES (All Locations):
  1. Layout: Required, alphanumeric, length 10
  2. C# Code: Must exist in crosswalk (line 87-93)
  3. SQL: NOT NULL constraint in table
  4. Output: Must be exactly 10 chars, left-aligned
  
  Cross-Check: All validations align ✓
```

---

## What Gets Documented for EVERY Field

```
For Each Field:
  1. Layout Validation Rules
  2. C# Property & Transformations
  3. Database Table & Column
  4. SQL Procedures Using It
  5. Output File Format
  6. Dependency Chain
  7. All Validation Rules
  8. Complete Examples

Result: COMPLETE KNOWLEDGE BASE
        Every field fully grounded
        Every transformation documented
        Every validation verified
```

---

## Business Benefits

### For HRP Implementation
- ✅ Complete understanding of each field
- ✅ Know where field goes in output
- ✅ Understand all validations
- ✅ Know what fields must come first
- ✅ Understand data transformations at each step

### For Maintenance
- ✅ Impact analysis when changing a field
- ✅ Validate changes across all layers
- ✅ Find all places using a field
- ✅ Trace data quality issues to source

### For Quality Assurance
- ✅ Validate against layout requirements
- ✅ Test all validation rules
- ✅ Verify output formatting
- ✅ Test with dependency chains

---

## Implementation Timeline

| Phase | Task | Duration |
|-------|------|----------|
| **Phase 1** | Add Layout Validation Agent | 1 week |
| **Phase 2** | Add Database Schema Agent | 1 week |
| **Phase 3** | Add Output Formatting Agent | 1 week |
| **Phase 4** | Add Dependency Chain Agent | 1 week |
| **Phase 5** | Add Validation Consolidator Agent | 1 week |
| **Phase 6** | Update Excel Templates (12 sheets) | 1 week |
| **Phase 7** | Update Documentation | 1 week |
| **Phase 8** | Testing & Validation | 1 week |

**Total: ~8 weeks to full comprehensive grounding**

---

## Version Evolution

- **v1.0.0**: Basic skill (5 agents, layout + code + SQL)
- **v2.0.0**: Enhanced skill (8 agents, complete field journey)
- **v3.0.0**: Advanced features (impact analysis, change tracking)

---

**Status**: Ready for enhancement to v2.0.0
