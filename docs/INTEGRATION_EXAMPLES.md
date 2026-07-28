# Integration Examples - Real-World Scenarios

**Version**: 1.0.0 | **Practical Implementation Patterns**

---

## Scenario 1: Single Vendor Implementation

**Goal**: Document one vendor for HRP implementation

```bash
/generate-business-rules
vendor_name: AnthemWGS_Claims
domain_type: Claims
layout_file_path: {PROJECT_ROOT}/HRP/AnthemWGS_Claims/OriginalVendorLayouts/Anthem_WGS_835_Mapping.xlsx
codebase_root: {PROJECT_ROOT}/Codebase/
c_sharp_extractor_path: AccumulatorExtractor/.../AnthemWgs/*.cs
sql_root_path: DB_.../StoredProcedures/
```

**Output Files:**
- Business_Rules_AnthemWGS_Claims.xlsx (implementation guide)
- README.md (how to use the documentation)
- Validation reports (accuracy verification)

**Next Step:** Review with HRP team, implement

---

## Scenario 2: Batch Processing All Vendors

**Goal**: Document all 287 vendors systematically

```bash
# Process 20 vendors per day in parallel
for day in {1..15}; do
  /generate-business-rules
  batch_mode: true
  vendors: [list of 20 vendors]
  output_directory: {PROJECT_ROOT}/HRP Delivery/$(date +%Y-%m-%d)/
done
```

**Timeline**: ~15 days for complete organization

**Tracking**: Monitor coverage percentage daily

---

## Scenario 3: Ad-Hoc Questions During Implementation

**Goal**: Answer specific implementation questions

```bash
/q-and-a
vendor_name: Navitus_Eligibility
codebase_root: {PROJECT_ROOT}/Codebase/
questions:
  - "How is MemberID formatted in the output?"
  - "Which fields use crosswalks?"
  - "Are there any hard-coded defaults?"
```

**Response Time**: Real-time (5-10 seconds)

---

## Scenario 4: Generate Formal Specification

**Goal**: Create documentation for business stakeholders

```bash
/generate-business-rules
vendor_name: BCBSAZJA_Claims
workflow: specification-generation
spec_type: business
output_format: docx
```

**Output**: Word document with:
- Executive summary
- Field descriptions (business language)
- Business rules
- Validation requirements

---

## Scenario 5: Debug Unmapped Fields

**Goal**: Investigate why fields aren't in code

```bash
# Step 1: Generate code mapping only
/generate-business-rules
vendor_name: [VendorName]
workflow: code-mapping-only

# Step 2: Review unmapped fields report

# Step 3: Ask specific questions
/q-and-a
vendor_name: [VendorName]
question: "Where is field 'FieldName' used in the code?"
```

**Resolution**: Identify if field is:
- Truly unmapped (not implemented)
- Named differently in code
- Calculated from other fields
- Not needed for output

---

**See USER_GUIDE.md for more workflows**
