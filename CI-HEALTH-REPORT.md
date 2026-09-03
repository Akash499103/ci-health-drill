# CI Health Report

## 1. Executive Summary

The CI pipeline is unhealthy and does not provide a reliable ship/no-ship signal. Across the 30-run historical window, 29 runs failed and only 1 passed, resulting in a 96.7% failure rate. Most failures were caused by CI configuration rather than application-code changes.

The top three risks are the missing dependency installation in the test job, the disabled Security Scan, and the flaky gateway integration test that depends on a live external service.

## 2. Workflow Observations

### Observation 1 — Test Job Dependency Failure

**Finding:** The test job runs `npm test` without installing dependencies.

**Evidence:** 25 of 30 historical runs showed the recurring error:

```text
jest: not found
Error: Process completed with exit code 127.