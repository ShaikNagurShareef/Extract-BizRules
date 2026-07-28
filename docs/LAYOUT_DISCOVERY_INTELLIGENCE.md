# Layout Discovery Intelligence - Multi-Sheet Excel Processing

**Version**: 2.0.0 | **Smart Layout Sheet Discovery & Classification**

---

## The Problem: Non-Standardized Layout Files

Vendor layout files are **chaotic**:

```
AnthemWGS_Claims_Mapping.xlsx has:
  ├── Sheet1: Field definitions (columns: Position, Length, Type)
  ├── Sheet2: Client variations (columns: Client, Field, Value)
  ├── Sheet3: Crosswalk mappings (columns: Source, Target)
  ├── Sheet4: Examples
  ├── Sheet5: Notes
  └── ... 10 more sheets with different purposes

MediKeeper_Eligibility_Mapping.xlsx has:
  ├── Mapping: Field definitions
  ├── Variation_ABA: ABA-specific changes
  ├── Variation_ACBL: ACBL-specific changes
  ├── Variation_ALCA: ALCA-specific changes
  ├── Crosswalks
  ├── Sample_Data
  └── ... completely different structure

BCBSAZJA_Claims_Mapping.xlsx has:
  ├── All_Fields: Every field definition
  ├── Header: Header record fields
  ├── Detail: Detail record fields
  ├── Trailer: Trailer record fields
  ├── Group_A: Group A-specific rules
  ├── Group_B: Group B-specific rules
  └── ... another different structure
```

**Result**: Current skill can't handle multi-sheet files properly

---

## NEW AGENT: Layout Discovery & Classification Engine

**Agent 9: Layout Sheet Discovery Agent** (NEW)

### What It Does

```
INPUT: Any Excel layout file (multi-sheet)

STEP 1: SHEET DISCOVERY
  - List all sheets
  - Detect sheet type by name pattern
  - Analyze sheet structure
  - Classify sheet purpose

STEP 2: SHEET CLASSIFICATION
  Map each sheet to one of:
  ✓ Main Field Definitions
  ✓ Client/Group Variations
  ✓ Crosswalk Mappings
  ✓ Field Examples
  ✓ Validation Rules
  ✓ Notes/Comments
  ✓ Sample Data
  ✓ Other

STEP 3: INTELLIGENT PARSING
  For each sheet type, extract:
  - Field definitions (auto-detect columns)
  - Variations (detect variation type)
  - Mappings (detect source/target)
  - Validation rules
  - Examples
  - Dependencies

STEP 4: STANDARDIZATION
  Convert non-standard sheets to:
  - Standard field definition format
  - Standard variation format
  - Standard mapping format
  - Standard validation format

STEP 5: MULTI-SHEET CONSOLIDATION
  - Merge all field definitions
  - Aggregate all variations
  - Consolidate all validations
  - Create master field list

OUTPUT: Standardized field data + variation map
```

---

## Sheet Type Detection Algorithm

### Pattern Recognition

```
Sheet Name Analysis:
  "Field*" or "Mapping*" or "Definition*" 
    → MAIN_FIELDS
  
  "Variation*" or "Client*" or "Group*" 
    → VARIATION
  
  "Crosswalk*" or "Lookup*" or "Map*" 
    → CROSSWALK
  
  "Example*" or "Sample*" 
    → EXAMPLES
  
  "Validation*" or "Rule*" 
    → VALIDATION_RULES
  
  "Note*" or "Comment*" 
    → NOTES

Structure Analysis:
  If contains columns: Position, Length, Type
    → MAIN_FIELDS
  
  If contains columns: Client, Field, Value
    → VARIATION
  
  If contains columns: Source, Target, Mapping
    → CROSSWALK
  
  If contains sample data
    → EXAMPLES
```

---

## Variation Detection & Client-Specific Data Correlation

### Multi-Variation Files with Client Correlation

Example: MediKeeper has separate sheets for each client variation

```
VARIATION SHEET PATTERNS:
  Sheet "Variation_ABA"
    → Client: ABA
    → Variations: Field mappings specific to ABA
  
  Sheet "Variation_ACBL"
    → Client: ACBL
    → Variations: Different field mappings for ACBL
  
  Sheet "Client_ABC"
    → Client: ABC
    → Variations: ABC-specific rules

VARIATION DETECTION ALGORITHM:
  1. Find all sheets matching variation patterns
  2. Extract client identifier from sheet name
  3. Extract field modifications for that client
  4. Consolidate into master variation map
  5. Document which fields vary by client
  6. Show examples for each variation
```

### CLIENT-SPECIFIC DATA CORRELATION (New Capability)

**Correlate data across tabs** to build unified client-variation understanding:

```
STEP 1: IDENTIFY MAIN FIELDS SHEET (BASELINE)
  ├─ Find sheet named "Mapping", "Fields", "Main", or similar
  ├─ Extract master field definitions (baseline for all clients)
  ├─ For ALL fields: name, position, type, length, required
  └─ Store as BASE_FIELDS

STEP 2: DISCOVER CLIENT VARIATION SHEETS
  ├─ Find all sheets matching variation patterns:
  │   ├─ "Client_[Name]"
  │   ├─ "Variation_[Name]"
  │   ├─ "[ClientName]_Specific"
  │   └─ Similar patterns
  ├─ Extract client name from sheet title
  └─ Store list of CLIENT_SHEETS = [ABA, ACBL, ALCA, ARSI, ...]

STEP 3: EXTRACT CLIENT-SPECIFIC MODIFICATIONS
  For each CLIENT_SHEET:
    ├─ Compare fields with BASE_FIELDS
    ├─ Identify differences (what changed for this client):
    │   ├─ Position changed? (e.g., 21-30 → 21-35)
    │   ├─ Length changed? (e.g., 10 chars → 12 chars)
    │   ├─ Type changed? (e.g., "A" → "N")
    │   ├─ Required flag changed? (Y → N or vice versa)
    │   ├─ Field removed? (exists in base, NOT in client)
    │   └─ Field added? (NOT in base, exists in client)
    ├─ Document modification type
    ├─ Show example (before → after)
    └─ Store as CLIENT_VARIATIONS[ClientName]

STEP 4: BUILD MASTER CORRELATION MATRIX
  Create unified view showing all clients together:
  
  ┌────────────┬──────────────┬──────────────┬──────────────┐
  │ Field Name │ Anthem(Base) │ ABA(Client)  │ ACBL(Client) │
  ├────────────┼──────────────┼──────────────┼──────────────┤
  │ ClaimID    │ 1-20,A       │ 1-20,A       │ 1-20,A       │
  │            │ (baseline)   │ (NO CHANGE)  │ (NO CHANGE)  │
  ├────────────┼──────────────┼──────────────┼──────────────┤
  │ GroupNum   │ 21-30,A,L10  │ 21-32,A,L12  │ 21-30,A,L10  │
  │            │ (baseline)   │ ⚠ LEN+2      │ (NO CHANGE)  │
  ├────────────┼──────────────┼──────────────┼──────────────┤
  │ MemberNum  │ 31-48,A,L18  │ 33-50,A,L18  │ 31-48,A,L18  │
  │            │ (baseline)   │ ⚠ POS MOVED  │ (NO CHANGE)  │
  └────────────┴──────────────┴──────────────┴──────────────┘

STEP 5: IDENTIFY FIELDS THAT VARY BY CLIENT
  ├─ Position-shifting fields: [GroupNum (ABA), MemberNum (ABA)]
  ├─ Length-changing fields: [GroupNum (ABA)]
  ├─ Type-changing fields: []
  ├─ Client-only fields: [ABACode (ABA only), SubGroup (ACBL only)]
  ├─ Fields removed by client: []
  └─ Stable fields (all clients): [ClaimID, EffDate, TermDate]

STEP 6: CONSOLIDATE CLIENT-SPECIFIC VIEW
  For each client (ABA, ACBL, ALCA, ...):
    ├─ Show fields that differ from baseline
    ├─ Show exact changes (positions, lengths, types)
    ├─ Show examples of impact (input → output)
    ├─ Document any client-only fields
    └─ Create client-specific field list
```

### Client-Specific Data Extraction Example

**Input File**: MediKeeper_Eligibility_Mapping.xlsx

**Sheet 1: Mapping** (Baseline for ALL clients)
```
| Field Name | Pos   | Length | Type | Required |
|------------|-------|--------|------|----------|
| MemberID  | 1-15  | 15     | A    | Y        |
| GroupNum  | 16-25 | 10     | A    | Y        |
| EffDate   | 26-33 | 8      | D    | Y        |
| TermDate  | 34-41 | 8      | D    | N        |
```

**Sheet 2: Variation_ABA** (ABA-specific modifications)
```
| Field Name | Pos   | Length | Type | Required | Diff from Base  |
|------------|-------|--------|------|----------|-----------------|
| MemberID  | 1-15  | 15     | A    | Y        | ✓ NO CHANGE     |
| GroupNum  | 16-26 | 11     | A    | Y        | ⚠ Length 10→11  |
| EffDate   | 27-34 | 8      | D    | Y        | ⚠ Pos 26→27     |
| TermDate  | 35-42 | 8      | D    | N        | ⚠ Pos 34→35     |
| ABACode   | 43-45 | 3      | A    | Y        | ⚠ NEW FIELD     |
```

**Sheet 3: Variation_ACBL** (ACBL-specific modifications)
```
| Field Name | Pos   | Length | Type | Required | Diff from Base  |
|------------|-------|--------|------|----------|-----------------|
| MemberID  | 1-15  | 15     | A    | Y        | ✓ NO CHANGE     |
| GroupNum  | 16-25 | 10     | A    | Y        | ✓ NO CHANGE     |
| EffDate   | 26-33 | 8      | D    | Y        | ✓ NO CHANGE     |
| TermDate  | 34-41 | 8      | D    | N        | ✓ NO CHANGE     |
| SubGroup  | 42-45 | 4      | A    | N        | ⚠ NEW FIELD     |
```

**CORRELATED OUTPUT**: Master Client Variation Map
```json
{
  "vendor": "MediKeeper",
  "domain": "Eligibility",
  "baseline": "Mapping",
  "clients": ["ABA", "ACBL"],
  
  "master_fields": [
    {
      "name": "MemberID",
      "baseline": {
        "position": "1-15",
        "length": 15,
        "type": "A",
        "required": true
      },
      "variations": {
        "ABA": { "position": "1-15", "length": 15, "type": "A" },
        "ACBL": { "position": "1-15", "length": 15, "type": "A" }
      },
      "varies_by_client": false,
      "clients_affected": []
    },
    {
      "name": "GroupNum",
      "baseline": {
        "position": "16-25",
        "length": 10,
        "type": "A",
        "required": true
      },
      "variations": {
        "ABA": {
          "position": "16-26",
          "length": 11,
          "type": "A",
          "changes": { "length": "10 → 11", "position": "25 → 26" }
        },
        "ACBL": { "position": "16-25", "length": 10, "type": "A" }
      },
      "varies_by_client": true,
      "clients_affected": ["ABA"],
      "impact": "⚠ ABA receives extra character in GroupNum (length +1)"
    },
    {
      "name": "ABACode",
      "baseline": null,
      "variations": {
        "ABA": { "position": "43-45", "length": 3, "type": "A" },
        "ACBL": null
      },
      "varies_by_client": true,
      "clients_affected": ["ABA"],
      "impact": "⚠ ABA ONLY field at position 43-45"
    }
  ],
  
  "client_variation_summary": {
    "ABA": {
      "differs_from_baseline": true,
      "modified_fields": ["GroupNum", "EffDate", "TermDate"],
      "new_fields": ["ABACode"],
      "removed_fields": [],
      "position_shifts": ["EffDate (26→27)", "TermDate (34→35)"],
      "length_changes": ["GroupNum (10→11)"],
      "impact_examples": [
        { "field": "GroupNum", "impact": "Length 10 vs 11 chars" },
        { "field": "ABACode", "impact": "Extra field at end" }
      ]
    },
    "ACBL": {
      "differs_from_baseline": true,
      "modified_fields": [],
      "new_fields": ["SubGroup"],
      "removed_fields": [],
      "position_shifts": [],
      "length_changes": [],
      "impact_examples": [
        { "field": "SubGroup", "impact": "Extra field at position 42-45" }
      ]
    }
  }
}
```

### Handling Complex Client Variation Patterns

**Different variation strategies across vendors**:

```
ANTHEM (separate sheet per client):
  Mapping (base)
  ├─ Client_ABA (full field list for ABA)
  ├─ Client_ACBL (full field list for ACBL)
  └─ Client_ALCA (full field list for ALCA)

MEDKEEPER (differences-only approach):
  Mapping (base)
  ├─ Variation_ABA (only changed/new fields)
  ├─ Variation_ACBL (only changed/new fields)
  └─ Variation_ALCA (only changed/new fields)

BCBSAZJA (record-type + client approach):
  Mapping (base for Header/Detail/Trailer)
  ├─ Header_ClientA (header for ClientA)
  ├─ Detail_ClientA (detail for ClientA)
  ├─ Header_ClientB (header for ClientB)
  └─ Detail_ClientB (detail for ClientB)

SOLUTION:
  1. Detect pattern type first (full vs delta vs record-type-based)
  2. Apply pattern-specific extraction logic
  3. Normalize all to master correlation matrix
  4. Result: Same unified output regardless of input format
```

---

## Multi-Record-Type Files

Example: BCBSAZJA has separate sheets for Header, Detail, Trailer

```
RECORD TYPE DETECTION:
  Sheet "Header"
    → Record Type: Header
    → Fields: Header-specific fields
  
  Sheet "Detail"
    → Record Type: Detail
    → Fields: Detail-specific fields
  
  Sheet "Trailer"
    → Record Type: Trailer
    → Fields: Trailer-specific fields

ALGORITHM:
  1. Detect record type from sheet names
  2. Extract field definitions per record type
  3. Link fields to record types
  4. Show record structure and order
  5. Document field counts per type
```

---

## Standardized Output from Layout Discovery

### For ANY layout file format, produce:

```
STANDARDIZED OUTPUT:
  ✓ Master Field List (all fields from all sheets)
  ✓ Field-to-Sheet Map (where each field came from)
  ✓ Client Variation Map (which fields vary by client)
  ✓ Record Type Map (which fields in which record types)
  ✓ Crosswalk Definitions (all lookups)
  ✓ Validation Rules (all validations)
  ✓ Examples (sample data)
  ✓ Standardized Format

Example Output:
  {
    "master_fields": [
      {
        "name": "GroupNumber",
        "position": 21-30,
        "type": "A",
        "length": 10,
        "source_sheet": "Mapping",
        "record_types": ["Header", "Detail"],
        "variations": {
          "ABA": { "length": 12 },
          "ACBL": { "length": 10 }
        }
      },
      ...
    ],
    "record_types": {
      "Header": { "fields": [...] },
      "Detail": { "fields": [...] },
      "Trailer": { "fields": [...] }
    },
    "variations": {
      "ABA": { "modified_fields": [...] },
      "ACBL": { "modified_fields": [...] }
    },
    "crosswalks": [...],
    "validation_rules": [...]
  }
```

---

## Enhanced Skill with Layout Discovery

### Agent 9: Layout Discovery & Classification (NEW)
- Sheet discovery
- Sheet classification
- Intelligent parsing
- Variation detection
- Record type identification
- Standardization
- Multi-sheet consolidation

### Then 8 other agents work on standardized data:
- Agent 1: Layout Validation Extractor
- Agent 2: Code Tracer
- Agent 3: Database Schema Mapper
- Agent 4: SQL Transformer
- Agent 5: Output Formatter
- Agent 6: Dependency Mapper
- Agent 7: SQL Validator
- Agent 8: Consolidator

---

## Benefits of Layout Discovery Agent

✅ **Handles ANY layout format**
  - Single sheet files
  - Multi-sheet files
  - Client variation files
  - Record-type variation files
  - Non-standard sheet names

✅ **Standardizes non-standard formats**
  - Converts to consistent structure
  - All downstream agents see consistent data
  - No format-specific logic needed downstream

✅ **Discovers client variations automatically**
  - No manual configuration needed
  - Works with any variation pattern
  - Handles multiple variation types

✅ **Manages complexity**
  - 50+ sheets handled transparently
  - Variation detection automatic
  - Record type identification automatic

---

## Updated Skill Architecture (9 Agents)

```
AGENT 9: LAYOUT DISCOVERY & CLASSIFICATION
  INPUT: Raw Excel layout file(s)
  OUTPUT: Standardized field data
         Client variation map
         Record type map
         Crosswalk map
         Validation rules

         ↓
         ↓ (All agents receive standardized data)
         ↓

AGENT 1: Layout Validation Extractor
  Extract validation rules from standardized data
  
AGENT 2: Code Tracer (Enhanced)
  Map standardized fields to C# code
  
AGENT 3: Database Schema Mapper
  Map standardized fields to DB
  
AGENT 4: SQL Transformer
  Find SQL using standardized fields
  
AGENT 5: Output Formatter
  Map standardized fields to output
  
AGENT 6: Dependency Mapper
  Find dependencies in standardized data
  
AGENT 7: Validation Consolidator
  Consolidate all validations
  
AGENT 8: README Generator
  Create documentation from all data

         ↓
         ↓
         ↓

OUTPUT: Complete Business Rules Excel (12 sheets)
        All client variations documented
        All record types documented
        Complete field tracing
```

---

## Implementation

### Phase 1: Layout Discovery Agent
- Detect sheet types
- Parse variations
- Standardize formats
- Create unified view

### Phase 2: Integration
- Feed standardized data to other agents
- Update all downstream agents
- Test with multi-sheet files

### Phase 3: Validation
- Test with 10+ vendor layout formats
- Ensure all variations captured
- Verify standardization accuracy

---

## Result

**ANY vendor layout file** → Automatically discovered and processed → Complete standardized Business Rules

No manual configuration needed. No format-specific code. Pure intelligence-based layout discovery.

---

**Status**: Critical enhancement for production skill  
**Priority**: HIGH  
**Impact**: Handles real-world layout chaos automatically
