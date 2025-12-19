# BjyAuthorize Dependency Audit Report

**Date:** 2025-12-19
**Analyzed File:** composer.json

---

## Executive Summary

This audit reveals **critical issues** with the project's dependencies. The project relies on:
- **Abandoned packages** (Zend Framework is now Laminas)
- **Security vulnerabilities** (CVE-2021-3007 in zend-http, CVE-2017-9841 in PHPUnit)
- **Extremely outdated versions** (PHP 5.3, PHPUnit 3.7)
- **Deprecated/abandoned dev tools**

**Recommendation:** This project requires a significant modernization effort to be safely used in production.

---

## Critical Issues

### 1. Zend Framework is Abandoned

**All** `zendframework/*` packages are **abandoned** and no longer receive security updates. Zend Framework was transferred to the [Laminas Project](https://getlaminas.org/) at the end of 2019.

| Current Package | Replacement (Laminas) |
|-----------------|----------------------|
| `zendframework/zend-permissions-acl` | `laminas/laminas-permissions-acl` |
| `zendframework/zend-mvc` | `laminas/laminas-mvc` |
| `zendframework/zend-eventmanager` | `laminas/laminas-eventmanager` |
| `zendframework/zend-servicemanager` | `laminas/laminas-servicemanager` |
| `zendframework/zend-http` | `laminas/laminas-http` |
| `zendframework/zend-view` | `laminas/laminas-view` |
| `zendframework/zend-cache` | `laminas/laminas-cache` |

**Migration Tool:** https://github.com/laminas/laminas-migration

---

## Security Vulnerabilities

### CVE-2021-3007 - Remote Code Execution (zend-http)
- **Severity:** HIGH (CVSS 9.8)
- **Affected:** `zendframework/zend-http` (all versions)
- **Description:** Deserialization vulnerability in `Zend\Http\Response\Stream` class can lead to remote code execution if untrusted content is deserialized.
- **Fix:** Migrate to `laminas/laminas-http` version 2.14.2+
- **Reference:** [Snyk Advisory](https://security.snyk.io/vuln/SNYK-PHP-ZENDFRAMEWORKZENDHTTP-1055259)

### CVE-2017-9841 - Remote Code Execution (PHPUnit)
- **Severity:** CRITICAL (CVSS 9.8)
- **Affected:** PHPUnit < 4.8.28, < 5.6.3 (version 3.7 is vulnerable)
- **Description:** Remote attackers can execute arbitrary PHP code via HTTP POST to `eval-stdin.php` if vendor folder is web-accessible.
- **Status:** In CISA Known Exploited Vulnerabilities list
- **Fix:** Upgrade to PHPUnit 10.x+
- **Reference:** [Acunetix Advisory](https://www.acunetix.com/vulnerabilities/web/phpunit-remote-code-execution/)

---

## Outdated Dependencies

### Production Dependencies (`require`)

| Package | Current Constraint | Issue | Recommended |
|---------|-------------------|-------|-------------|
| `php` | `>=5.3.3` | PHP 5.3 EOL: Aug 2014, PHP 5.6 EOL: Dec 2018 | `>=8.1` |
| `zendframework/*` | `~2.2` | Abandoned, no security updates | Migrate to Laminas |

### Development Dependencies (`require-dev`)

| Package | Current | Latest | Issue |
|---------|---------|--------|-------|
| `phpunit/phpunit` | `~3.7` | `11.x` | 10+ years outdated, CVE-2017-9841 |
| `doctrine/common` | `>=2.3,<2.5-dev` | `3.x` | Outdated constraint |
| `zendframework/zend-developer-tools` | `0.*` | Abandoned | Use `laminas/laminas-developer-tools` |
| `zf-commons/zfc-user` | `1.*` | Abandoned | Community fork needed |
| `squizlabs/php_codesniffer` | `1.4.*` | `4.0.x` | Extremely outdated, repo abandoned |
| `satooshi/php-coveralls` | `~0.6` | Abandoned | Use `php-coveralls/php-coveralls` |

---

## Unnecessary Bloat

### 1. Overly Permissive Version Constraints
The use of `~2.2` for all Zend packages locks to very old versions. Modern practices use caret (`^`) for semver compatibility.

### 2. `minimum-stability: dev`
Setting `"minimum-stability": "dev"` allows unstable packages, increasing risk. Remove or set to `"stable"` with specific dev requirements using `@dev` flag where needed.

---

## Recommendations

### Immediate Actions (Security Critical)

1. **Migrate from Zend Framework to Laminas**
   ```bash
   composer require laminas/laminas-migration
   ./vendor/bin/laminas-migration migrate
   ```

2. **Update PHP requirement**
   ```json
   "require": {
       "php": ">=8.1"
   }
   ```

3. **Update PHPUnit**
   ```json
   "require-dev": {
       "phpunit/phpunit": "^10.0 || ^11.0"
   }
   ```

### Recommended composer.json Updates

```json
{
    "require": {
        "php": ">=8.1",
        "laminas/laminas-permissions-acl": "^2.10",
        "laminas/laminas-mvc": "^3.6",
        "laminas/laminas-eventmanager": "^3.10",
        "laminas/laminas-servicemanager": "^3.22",
        "laminas/laminas-http": "^2.18",
        "laminas/laminas-view": "^2.27",
        "laminas/laminas-cache": "^3.10"
    },
    "require-dev": {
        "phpunit/phpunit": "^10.0 || ^11.0",
        "doctrine/common": "^3.4",
        "laminas/laminas-developer-tools": "^2.8",
        "squizlabs/php_codesniffer": "^3.7",
        "php-coveralls/php-coveralls": "^2.5"
    }
}
```

### Migration Checklist

- [ ] Run Laminas migration tool
- [ ] Update namespace references (`Zend\` → `Laminas\`)
- [ ] Update configuration files
- [ ] Update PHP version in CI/CD
- [ ] Run test suite with new dependencies
- [ ] Remove `zf-commons/zfc-user` or find community fork
- [ ] Update `.travis.yml` or GitHub Actions for PHP 8.x
- [ ] Update documentation to reflect Laminas

---

## Sources

- [Laminas Migration Documentation](https://docs.laminas.dev/migration/)
- [Zend to Laminas Retrospective](https://getlaminas.org/blog/2020-03-09-transferring-zf-to-laminas.html)
- [CVE-2021-3007 (zend-http)](https://security.snyk.io/vuln/SNYK-PHP-ZENDFRAMEWORKZENDHTTP-1055259)
- [CVE-2017-9841 (PHPUnit)](https://www.acunetix.com/vulnerabilities/web/phpunit-remote-code-execution/)
- [PHP_CodeSniffer Repository](https://github.com/squizlabs/PHP_CodeSniffer)
- [php-coveralls Migration](https://github.com/php-coveralls/php-coveralls/issues/260)

---

## Risk Assessment

| Category | Risk Level | Impact |
|----------|------------|--------|
| Security Vulnerabilities | **CRITICAL** | Remote code execution possible |
| Outdated Dependencies | **HIGH** | No security patches available |
| Maintainability | **HIGH** | No upstream support |
| Compatibility | **MEDIUM** | Won't work with PHP 8.x |

**Overall Risk: CRITICAL** - This project should not be used in production without significant updates.
