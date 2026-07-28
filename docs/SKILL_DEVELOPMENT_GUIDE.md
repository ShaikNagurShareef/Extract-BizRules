# Skill Development Guide - Extending Extract-BizRules

**For**: Contributors and developers extending the skill  
**Version**: 1.0.0 | **Status**: Complete reference for contributors

---

## How to Extend the Skill

### Adding a New Agent

1. **Define Agent Purpose** - What specific task does it perform?
2. **Design Inputs** - What data does it need from other agents?
3. **Design Outputs** - What structure should it produce?
4. **Implement Logic** - Code to find and extract data
5. **Add Validation** - Verify output completeness
6. **Integrate into Workflow** - Add to orchestration
7. **Document** - Update architecture docs

### Adding Support for New Vendor

1. **Identify C# Extractor Location**
   - Find vendor code: `{CODEBASE_ROOT}/[Extractor]/src/PbmRecords/[Vendor]/*.cs`
2. **Identify Record Classes**
   - HeaderRecord, DetailRecord, TrailerRecord
3. **Map Properties** - Extract from C# code
4. **Add to Vendor Registry** - Document in code
5. **Test with Sample** - Run skill with new vendor
6. **Validate Accuracy** - Spot-check against actual code

### Quality Standards for Extensions

- ✅ No hallucinated data
- ✅ Every claim traceable to source
- ✅ Exact code locations (file + line)
- ✅ Business logic explained
- ✅ Complete documentation

---

## Contributing Improvements

1. **Fork the repository**
2. **Create feature branch**: `git checkout -b feature/your-improvement`
3. **Make changes** with clear commits
4. **Test thoroughly** against multiple vendors
5. **Document changes** in README/architecture docs
6. **Submit pull request** with detailed description

---

**For full development details**: See SKILL_ARCHITECTURE.md Extensibility section
