# BjyAuthorize Remediation Cost Estimate

**Date:** 2025-12-19
**Based on:** REMEDIATION_PLAN.md

---

## Executive Summary

| Scenario | Hours | Cost Range (USD) |
|----------|-------|------------------|
| **Full Modernization** | 40-64 hours | **$3,200 - $8,000** |
| **Minimal Security Fix** | 12-20 hours | **$960 - $2,500** |

---

## Hourly Rate Assumptions

| Role | Rate Range (USD/hr) |
|------|---------------------|
| Junior PHP Developer | $50 - $75 |
| Mid-level PHP/Laminas Developer | $75 - $100 |
| Senior PHP/Laminas Specialist | $100 - $150 |
| **Assumed Average** | **$80 - $125** |

*Note: Rates vary significantly by region and contractor vs. agency.*

---

## Full Modernization Estimate

### Phase-by-Phase Breakdown

| Phase | Description | Files | Hours (Low) | Hours (High) |
|-------|-------------|-------|-------------|--------------|
| **1** | Dependency updates (composer.json) | 1 | 1 | 2 |
| **2** | Namespace migration (automated + verification) | 44 | 3 | 5 |
| **3** | PHPUnit 3.7 → 10.x migration | 32 | 12 | 20 |
| **4** | CI/CD (Travis → GitHub Actions) | 2 | 2 | 3 |
| **5** | Configuration updates | 4 | 2 | 3 |
| **6** | Factory interface updates (PSR-11) | 21 | 10 | 16 |
| | **Subtotal Development** | | **30** | **49** |

### Additional Effort

| Task | Hours (Low) | Hours (High) |
|------|-------------|--------------|
| Testing & debugging | 6 | 10 |
| Code review | 2 | 3 |
| Documentation updates | 2 | 2 |
| **Subtotal Additional** | **10** | **15** |

### Total Hours

| | Low | High |
|-|-----|------|
| Development | 30 | 49 |
| Additional | 10 | 15 |
| **Total** | **40** | **64** |

### Cost Calculation

| Rate Tier | Low Hours (40) | High Hours (64) |
|-----------|----------------|-----------------|
| Junior ($50-75/hr) | $2,000 - $3,000 | $3,200 - $4,800 |
| Mid-level ($75-100/hr) | $3,000 - $4,000 | $4,800 - $6,400 |
| Senior ($100-150/hr) | $4,000 - $6,000 | $6,400 - $9,600 |
| **Blended Average ($80-125/hr)** | **$3,200 - $5,000** | **$5,120 - $8,000** |

---

## Minimal Security Fix Estimate

*Patches only critical CVEs without full modernization*

| Task | Hours (Low) | Hours (High) |
|------|-------------|--------------|
| Fork/patch zend-http (CVE-2021-3007) | 4 | 6 |
| Update PHPUnit to 4.8.28+ | 4 | 8 |
| Update CI for PHP 5.6/7.x | 2 | 3 |
| Testing | 2 | 3 |
| **Total** | **12** | **20** |

### Cost Calculation

| Rate Tier | Low Hours (12) | High Hours (20) |
|-----------|----------------|-----------------|
| Junior ($50-75/hr) | $600 - $900 | $1,000 - $1,500 |
| Mid-level ($75-100/hr) | $900 - $1,200 | $1,500 - $2,000 |
| Senior ($100-150/hr) | $1,200 - $1,800 | $2,000 - $3,000 |
| **Blended Average ($80-125/hr)** | **$960 - $1,500** | **$1,600 - $2,500** |

---

## Detailed Task Breakdown

### Phase 3: PHPUnit Migration (Highest Effort)

| Task | Files | Est. Hours |
|------|-------|------------|
| Update base test class imports | 32 | 2 |
| Replace `getMock()` → `createMock()` | ~20 | 3-5 |
| Replace `setMethods()` → `onlyMethods()` | ~15 | 2-4 |
| Update exception testing syntax | ~10 | 2-3 |
| Fix assertion deprecations | ~25 | 2-3 |
| Debug test failures | - | 3-5 |
| **Subtotal** | | **12-20** |

### Phase 6: Factory Interface Migration

| Task | Files | Est. Hours |
|------|-------|------------|
| Update interface imports | 21 | 1-2 |
| Rename `createService()` → `__invoke()` | 21 | 4-6 |
| Update method signatures (PSR-11) | 21 | 3-5 |
| Fix service locator calls | 21 | 2-3 |
| **Subtotal** | | **10-16** |

---

## Cost Factors & Risks

### Factors That Increase Cost

| Factor | Impact | Probability |
|--------|--------|-------------|
| Laminas 3.x breaking changes beyond docs | +20-40% | Medium |
| zfc-user dependency requires forking | +8-16 hrs | High |
| Hidden integration issues | +10-20% | Medium |
| Incomplete test coverage reveals bugs | +5-15% | Low |

### Factors That Decrease Cost

| Factor | Impact | Notes |
|--------|--------|-------|
| laminas-migration tool works perfectly | -20% | Likely for namespace changes |
| Developer familiar with ZF→Laminas | -15-25% | Reduces learning curve |
| Rector PHP available for PHPUnit | -10-15% | Automated refactoring |

---

## Alternative Pricing Models

### Fixed Price Quote

| Scenario | Fixed Price Range |
|----------|-------------------|
| Full Modernization | $4,500 - $7,500 |
| Minimal Security Fix | $1,200 - $2,000 |

*Fixed price typically includes 15-25% buffer for scope creep*

### Retainer/Time & Materials

| Monthly Retainer | Hours Included | Completion Time |
|------------------|----------------|-----------------|
| $2,000/month | 20-25 hrs | 2-3 months |
| $4,000/month | 40-50 hrs | 1-2 months |

---

## ROI Consideration

### Cost of NOT Remediating

| Risk | Potential Cost |
|------|----------------|
| Security breach via CVE-2021-3007 | $10,000 - $1,000,000+ |
| Security breach via CVE-2017-9841 | $10,000 - $1,000,000+ |
| Inability to upgrade PHP (hosting costs, performance) | $500 - $5,000/year |
| Technical debt accumulation | $2,000 - $10,000/year |
| Developer productivity loss (outdated tooling) | $1,000 - $5,000/year |

### Break-Even Analysis

At the **low estimate of $3,200**, the remediation pays for itself if it prevents:
- One minor security incident, OR
- 1-2 years of accumulated technical debt costs, OR
- 3-6 months of developer productivity loss

---

## Recommended Approach

### Option A: Full Modernization (Recommended)
- **Cost:** $3,200 - $8,000
- **Timeline:** 1-2 weeks (dedicated developer)
- **Outcome:** Modern, secure, maintainable codebase

### Option B: Minimal Security Fix
- **Cost:** $960 - $2,500
- **Timeline:** 2-4 days
- **Outcome:** Critical CVEs patched, but technical debt remains

### Option C: Replace Package
- **Cost:** Variable (depends on alternative)
- **Consider if:** Project has minimal customization
- **Alternatives:** laminas/laminas-permissions-acl with custom guards

---

## Summary

| Metric | Full Modernization | Minimal Fix |
|--------|-------------------|-------------|
| **Estimated Hours** | 40-64 | 12-20 |
| **Cost Range** | $3,200 - $8,000 | $960 - $2,500 |
| **Risk Reduction** | High | Medium |
| **Future Maintenance** | Low | High |
| **PHP 8.x Support** | Yes | No |
| **Recommendation** | **Preferred** | Stopgap only |
