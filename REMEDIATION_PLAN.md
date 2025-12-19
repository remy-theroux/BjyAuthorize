# BjyAuthorize Remediation Plan

**Date:** 2025-12-19
**Scope:** Full modernization from Zend Framework to Laminas

---

## Summary of Changes Required

| Category | Items | Files Affected |
|----------|-------|----------------|
| Namespace migrations | 142 occurrences | 44 files |
| Test file updates | PHPUnit API changes | 32 test files |
| Configuration updates | 4 config files | 4 files |
| Documentation updates | 3 markdown files | 3 files |
| **Total files to modify** | | **83 files** |

---

## Phase 1: Dependency Updates (composer.json)

### Current → Target Versions

| Package | Current | Target | Change Type |
|---------|---------|--------|-------------|
| `php` | `>=5.3.3` | `>=8.1` | Update |
| `zendframework/zend-permissions-acl` | `~2.2` | `laminas/laminas-permissions-acl: ^2.10` | Replace |
| `zendframework/zend-mvc` | `~2.2` | `laminas/laminas-mvc: ^3.6` | Replace |
| `zendframework/zend-eventmanager` | `~2.2` | `laminas/laminas-eventmanager: ^3.10` | Replace |
| `zendframework/zend-servicemanager` | `~2.2` | `laminas/laminas-servicemanager: ^3.22` | Replace |
| `zendframework/zend-http` | `~2.2` | `laminas/laminas-http: ^2.18` | Replace |
| `zendframework/zend-view` | `~2.2` | `laminas/laminas-view: ^2.27` | Replace |
| `zendframework/zend-cache` | `~2.2` | `laminas/laminas-cache: ^3.10` | Replace |

### Dev Dependencies

| Package | Current | Target | Change Type |
|---------|---------|--------|-------------|
| `phpunit/phpunit` | `~3.7` | `^10.0` | Update |
| `doctrine/common` | `>=2.3,<2.5-dev` | `^3.4` | Update |
| `zendframework/zend-developer-tools` | `0.*` | `laminas/laminas-developer-tools: ^2.8` | Replace |
| `zf-commons/zfc-user` | `1.*` | Remove or find fork | Evaluate |
| `squizlabs/php_codesniffer` | `1.4.*` | `^3.7` | Update |
| `satooshi/php-coveralls` | `~0.6` | `php-coveralls/php-coveralls: ^2.5` | Replace |

### Remediation Tasks

```json
{
    "name": "bjyoungblood/bjy-authorize",
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
        "phpunit/phpunit": "^10.0",
        "doctrine/common": "^3.4",
        "laminas/laminas-developer-tools": "^2.8",
        "squizlabs/php_codesniffer": "^3.7",
        "php-coveralls/php-coveralls": "^2.5"
    }
}
```

---

## Phase 2: Namespace Migration (Zend → Laminas)

### Breakdown by Namespace

| Namespace | Occurrences | Files | Search | Replace |
|-----------|-------------|-------|--------|---------|
| ServiceManager | 55 | 30 | `Zend\ServiceManager` | `Laminas\ServiceManager` |
| Permissions\Acl | 39 | 17 | `Zend\Permissions\Acl` | `Laminas\Permissions\Acl` |
| Mvc | 21 | 12 | `Zend\Mvc` | `Laminas\Mvc` |
| EventManager | 10 | 8 | `Zend\EventManager` | `Laminas\EventManager` |
| Cache | 8 | 5 | `Zend\Cache` | `Laminas\Cache` |
| Http | 6 | 6 | `Zend\Http` | `Laminas\Http` |
| View | 3 | 3 | `Zend\View` | `Laminas\View` |
| **Total** | **142** | **44** | | |

### Files Requiring Changes

#### Source Files (41 files)
```
src/BjyAuthorize/Acl/HierarchicalRoleInterface.php
src/BjyAuthorize/Acl/Role.php
src/BjyAuthorize/Collector/RoleCollector.php
src/BjyAuthorize/Controller/Plugin/IsAllowed.php
src/BjyAuthorize/Guard/AbstractGuard.php
src/BjyAuthorize/Guard/Controller.php
src/BjyAuthorize/Guard/GuardInterface.php
src/BjyAuthorize/Guard/Route.php
src/BjyAuthorize/Module.php
src/BjyAuthorize/Provider/Identity/AuthenticationIdentityProvider.php
src/BjyAuthorize/Provider/Identity/ProviderInterface.php
src/BjyAuthorize/Provider/Identity/ZfcUserZendDb.php
src/BjyAuthorize/Provider/Resource/Config.php
src/BjyAuthorize/Provider/Resource/ProviderInterface.php
src/BjyAuthorize/Provider/Role/Config.php
src/BjyAuthorize/Provider/Role/ObjectRepositoryProvider.php
src/BjyAuthorize/Provider/Role/ProviderInterface.php
src/BjyAuthorize/Provider/Role/ZendDb.php
src/BjyAuthorize/Service/AuthenticationIdentityProviderServiceFactory.php
src/BjyAuthorize/Service/Authorize.php
src/BjyAuthorize/Service/AuthorizeAwareServiceInitializer.php
src/BjyAuthorize/Service/AuthorizeFactory.php
src/BjyAuthorize/Service/BaseProvidersServiceFactory.php
src/BjyAuthorize/Service/CacheFactory.php
src/BjyAuthorize/Service/CacheKeyGeneratorFactory.php
src/BjyAuthorize/Service/ConfigResourceProviderServiceFactory.php
src/BjyAuthorize/Service/ConfigRoleProviderServiceFactory.php
src/BjyAuthorize/Service/ConfigRuleProviderServiceFactory.php
src/BjyAuthorize/Service/ConfigServiceFactory.php
src/BjyAuthorize/Service/ControllerGuardServiceFactory.php
src/BjyAuthorize/Service/IdentityProviderServiceFactory.php
src/BjyAuthorize/Service/ObjectRepositoryRoleProviderFactory.php
src/BjyAuthorize/Service/RoleCollectorServiceFactory.php
src/BjyAuthorize/Service/RouteGuardServiceFactory.php
src/BjyAuthorize/Service/UnauthorizedStrategyServiceFactory.php
src/BjyAuthorize/Service/UserRoleServiceFactory.php
src/BjyAuthorize/Service/ZendDbRoleProviderServiceFactory.php
src/BjyAuthorize/Service/ZfcUserZendDbIdentityProviderServiceFactory.php
src/BjyAuthorize/View/Helper/IsAllowed.php
src/BjyAuthorize/View/RedirectionStrategy.php
src/BjyAuthorize/View/UnauthorizedStrategy.php
```

#### Test Files (27 files)
```
tests/BjyAuthorizeTest/Collector/RoleCollectorTest.php
tests/BjyAuthorizeTest/Guard/ControllerTest.php
tests/BjyAuthorizeTest/Guard/RouteTest.php
tests/BjyAuthorizeTest/Provider/Identity/AuthenticationIdentityProviderTest.php
tests/BjyAuthorizeTest/Provider/Identity/ZfcUserZendDbTest.php
tests/BjyAuthorizeTest/Provider/Role/ZendDbTest.php
tests/BjyAuthorizeTest/Service/AuthorizeFactoryTest.php
tests/BjyAuthorizeTest/Service/AuthorizeTest.php
tests/BjyAuthorizeTest/Service/CacheFactoryTest.php
tests/BjyAuthorizeTest/Service/MockProvider.php
tests/BjyAuthorizeTest/Service/ObjectRepositoryRoleProviderFactoryTest.php
tests/BjyAuthorizeTest/View/RedirectionStrategyTest.php
tests/BjyAuthorizeTest/View/UnauthorizedStrategyTest.php
... (and 14 more test files for PHPUnit updates)
```

### Automated Migration Command

```bash
# Install Laminas migration tool
composer require --dev laminas/laminas-migration

# Run migration
./vendor/bin/laminas-migration migrate
```

---

## Phase 3: PHPUnit Migration (3.7 → 10.x)

### Breaking Changes to Address

| PHPUnit 3.x | PHPUnit 10.x | Files Affected |
|-------------|--------------|----------------|
| `PHPUnit_Framework_TestCase` | `PHPUnit\Framework\TestCase` | 32 files |
| `@expectedException` annotation | `$this->expectException()` | Review needed |
| `getMock()` | `createMock()` | Review needed |
| `setExpectedException()` | `expectException()` | Review needed |
| `getMockBuilder()->setMethods()` | `getMockBuilder()->onlyMethods()` | Review needed |

### phpunit.xml.dist Updates Required

```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="vendor/phpunit/phpunit/phpunit.xsd"
         bootstrap="tests/Bootstrap.php"
         colors="true"
         cacheDirectory=".phpunit.cache"
         executionOrder="depends,defects"
         beStrictAboutOutputDuringTests="true"
         failOnRisky="true"
         failOnWarning="true">
    <testsuites>
        <testsuite name="BjyAuthorize tests">
            <directory>tests/BjyAuthorizeTest</directory>
        </testsuite>
    </testsuites>
    <source>
        <include>
            <directory suffix=".php">src</directory>
        </include>
    </source>
</phpunit>
```

---

## Phase 4: CI/CD Updates

### Current .travis.yml Issues

- Uses PHP 5.3.3, 5.3, 5.4, 5.5 (all EOL)
- Uses HHVM (no longer supported)
- Travis CI has limited free tier

### Recommended: GitHub Actions

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        php-version: ['8.1', '8.2', '8.3']

    steps:
      - uses: actions/checkout@v4

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: ${{ matrix.php-version }}
          coverage: xdebug

      - name: Install dependencies
        run: composer install --prefer-dist --no-progress

      - name: Run tests
        run: vendor/bin/phpunit --coverage-clover coverage.xml

      - name: Run code sniffer
        run: vendor/bin/phpcs --standard=PSR12 src/ tests/

      - name: Upload coverage to Coveralls
        uses: coverallsapp/github-action@v2
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          file: coverage.xml
```

---

## Phase 5: Configuration Updates

### config/module.config.php

| Change | Line | Current | Target |
|--------|------|---------|--------|
| Alias | 89 | `Zend\Db\Adapter\Adapter` | `Laminas\Db\Adapter\Adapter` |
| Config key | 105 | `zenddevelopertools` | `laminas-developer-tools` |

### Documentation Updates

| File | Changes Required |
|------|------------------|
| `README.md` | Update all Zend references, installation instructions |
| `docs/doctrine.md` | Update namespace references |
| `docs/unauthorized-strategies.md` | Update namespace references |

---

## Phase 6: API Compatibility (Laminas 3.x)

### Breaking Changes in Laminas MVC 3.x

| Change | Impact | Action Required |
|--------|--------|-----------------|
| Factory interfaces changed | HIGH | Update all `*ServiceFactory` classes |
| ServiceManager v3 | HIGH | Update factory method signatures |
| EventManager v3 | MEDIUM | Update event attachment code |

### Factory Interface Migration

**Old (ZF2):**
```php
use Zend\ServiceManager\FactoryInterface;
use Zend\ServiceManager\ServiceLocatorInterface;

class MyFactory implements FactoryInterface
{
    public function createService(ServiceLocatorInterface $serviceLocator)
    {
        // ...
    }
}
```

**New (Laminas 3.x):**
```php
use Laminas\ServiceManager\Factory\FactoryInterface;
use Psr\Container\ContainerInterface;

class MyFactory implements FactoryInterface
{
    public function __invoke(ContainerInterface $container, $requestedName, ?array $options = null)
    {
        // ...
    }
}
```

**Files requiring factory updates:** 21 factory files in `src/BjyAuthorize/Service/`

---

## Remediation Task Checklist

### Priority 1: Security Critical
- [ ] Update `composer.json` with Laminas packages
- [ ] Run `composer update`
- [ ] Fix CVE-2021-3007 (zend-http → laminas-http)
- [ ] Fix CVE-2017-9841 (PHPUnit upgrade)

### Priority 2: Namespace Migration
- [ ] Install laminas-migration tool
- [ ] Run automated namespace migration
- [ ] Verify all `Zend\` → `Laminas\` changes
- [ ] Update config file references

### Priority 3: PHPUnit Modernization
- [ ] Update phpunit.xml.dist configuration
- [ ] Update test base classes
- [ ] Replace deprecated mock methods
- [ ] Replace deprecated assertions
- [ ] Update exception testing syntax

### Priority 4: Factory Interfaces
- [ ] Update all 21 factory files to PSR-11
- [ ] Update `FactoryInterface` implementations
- [ ] Test service container integration

### Priority 5: CI/CD
- [ ] Create GitHub Actions workflow
- [ ] Remove or archive .travis.yml
- [ ] Update code style to PSR-12
- [ ] Configure Coveralls for GitHub Actions

### Priority 6: Documentation
- [ ] Update README.md
- [ ] Update docs/doctrine.md
- [ ] Update docs/unauthorized-strategies.md
- [ ] Update inline code examples

---

## Effort Estimate

| Phase | Tasks | Complexity |
|-------|-------|------------|
| Phase 1: Dependencies | 1 file | Low |
| Phase 2: Namespaces | 44 files, 142 changes | Medium (automated) |
| Phase 3: PHPUnit | 32 files | High |
| Phase 4: CI/CD | 2 files | Low |
| Phase 5: Config | 4 files | Low |
| Phase 6: Factories | 21 files | High |

### Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Laminas API incompatibility | Medium | High | Thorough testing, staged rollout |
| PHPUnit test failures | High | Medium | Run tests iteratively |
| zfc-user dependency broken | High | Medium | Evaluate alternatives or fork |
| Hidden Zend dependencies | Low | Medium | Full grep verification |

---

## Execution Order

1. **Create feature branch** for modernization
2. **Update composer.json** (Phase 1)
3. **Run laminas-migration** (Phase 2)
4. **Manual namespace verification** (Phase 2)
5. **Update factory interfaces** (Phase 6)
6. **Update PHPUnit tests** (Phase 3)
7. **Update CI/CD** (Phase 4)
8. **Update configuration** (Phase 5)
9. **Run full test suite**
10. **Update documentation**
11. **Create pull request**

---

## Alternative: Minimal Security Fix

If full modernization is not feasible, a **minimal security fix** approach:

1. **Fork and patch zend-http** to fix CVE-2021-3007
2. **Update PHPUnit** to 4.8.28+ (minimal breaking changes)
3. **Keep PHP 5.6** minimum (still EOL but less breaking)

**Warning:** This approach leaves the project on abandoned packages without long-term support.
