# CI History Analysis

Repository: `ci-health-drill`  
Scope: Last 30 GitHub Actions workflow runs  
Workflows analyzed: `CI` and `Security Scan`

---

## Task 1 — Run History

| # | Date | Trigger commit | Outcome | First failing job | Failing step | Failure tied to code change? |
|---|---|---|---|---|---|---|
| 30 | 2026-05-27 | `aea721e` skip flaky gateway test again | ❌ | `test` | `npm test` | No — config |
| 29 | 2026-05-27 | `8850966` document edge cases | ❌ | `test` | `npm test` | No — config |
| 28 | 2026-05-27 | `e497451` add newline to readme | ❌ | `test` | `npm test` | No — docs-only change |
| 27 | 2026-05-27 | `2294e50` stricter token validation note | ❌ | `test` | `npm test` | No — config |
| 26 | 2026-05-27 | `2d90333` improve currency logging | ❌ | `test` | `npm test` | No — config |
| 25 | 2026-05-26 | `22d3b40` re-enable gateway integration test | ❌ | `test` | Gateway integration test | Yes — flaky network test |
| 24 | 2026-05-26 | `22d3b40` re-run | ✅ | — | — | Flaky — passed on retry |
| 23 | 2026-05-26 | `22d3b40` re-run | ❌ | `test` | Gateway integration test | Yes — flaky network test |
| 22 | 2026-05-25 | `4bcd7a9` clean up validate comments | ❌ | `test` | `npm test` | No — config |
| 21 | 2026-05-25 | `555475f` recheck token validation | ❌ | `test` | `npm test` | No — config |
| 20 | 2026-05-24 | `f51f14a` verify payment flow | ❌ | `test` | `npm test` | No — config |
| 19 | 2026-05-24 | `af8ec1c` temp: disable scan until fixed | ❌ | `test` | `npm test` | No — config |
| 18 | 2026-05-23 | `af8ec1c` security scan skipped | ❌ | `test` | `npm test` | No — config |
| 17 | 2026-05-23 | `30516e1` add debug logging | ❌ | `test` | `npm test` | No — config |
| 16 | 2026-05-22 | `4a70974` bump version to 1.0.1 | ❌ | `test` | `npm test` | No — config |
| 15 | 2026-05-22 | `0f4e90a` skip failing test for now | ❌ | `test` | `npm test` | No — config |
| 14 | 2026-05-21 | `0d9287c` quick auth patch | ❌ | `test` | `npm test` | No — config |
| 13 | 2026-05-21 | `0d9287c` re-run | ❌ | `test` | `npm test` | No — config |
| 12 | 2026-05-20 | `fbd120d` hotfix: urgent payment fix | ❌ | `test` | `npm test` | No — config |
| 11 | 2026-05-20 | `fbd120d` direct push to main | ❌ | `test` | `npm test` | No — config |
| 10 | 2026-05-19 | `0311ceb` payment platform CI setup | ❌ | `test` | `npm test` | No — config |
| 9 | 2026-05-19 | validateAmount edge case | ❌ | `test` | Gateway integration test | Yes — flaky network test |
| 8 | 2026-05-18 | routine push | ❌ | `test` | `npm test` | No — config |
| 7 | 2026-05-18 | tokenValidator stricter checks | ❌ | `test` | `npm test` | No — config |
| 6 | 2026-05-17 | processPayment log currency | ❌ | `test` | `npm test` | No — config |
| 5 | 2026-05-17 | routine push | ❌ | `test` | `npm test` | No — config |
| 4 | 2026-05-16 | validateAmount debug logging | ❌ | `test` | Gateway integration test | Yes — flaky network test |
| 3 | 2026-05-16 | tokenValidator trigger | ❌ | `test` | `npm test` | No — config |
| 2 | 2026-05-15 | processPayment trigger | ❌ | `test` | `npm test` | No — config |
| 1 | 2026-05-15 | `0311ceb` initial CI setup | ❌ | `install` + `test` | `npm test` | No — config |

### Security Scan

The `Security Scan` workflow does not appear in the 30-run history because it was hard-disabled using `if: false`.

This absence is itself a CI health finding.

---

## Failure Rate

Total runs analyzed: **30**

Failed runs: **29**

Passed runs: **1**

Failure rate:

**29 / 30 = 96.7%**

Success rate:

**1 / 30 = 3.3%**

The pipeline therefore provides an extremely poor ship/no-ship signal.

---

## Failures by Job

| Failing job | Root cause | Failures | Type |
|---|---|---:|---|
| `test` | `npm test` runs without dependencies installed; Jest is missing | 25 | Consistent / deterministic |
| `test` | Gateway integration test makes a real HTTP request | 4 | Flaky / non-deterministic |
| `install` | No useful dependency handoff to `test` | Structural | Configuration |
| `scan` | Security Scan disabled with `if: false` | N/A | Configuration / merge safety |

---

## Flaky vs Consistent

### Consistent failures — 25 of 29 failures

The `test` job runs `npm test` without installing dependencies.

The repeated error is:

```text
> ci-health-drill@1.0.1 test
> jest --forceExit

sh: 1: jest: not found
Error: Process completed with exit code 127.