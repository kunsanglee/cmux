Symptom: XCTest-launched cmux processes report intentional test crashes to the shared Sentry project.
User impact: 1,600 false fatal events in one group obscure production regressions and inflate the unresolved issue inventory.
Source: https://manaflow.sentry.io/issues/7588379387/
Target surface: macOS test host
Build/version/tag: development builds 0.64.17 through 0.64.20
Repro workload: run the app-hosted test suite that exercises a missing SidebarUnreadModel environment object.
Expected bad behavior: the test crashes locally and Sentry records a fatal event from `/tmp/cmux-xctest-*.sock`.

Evidence:
- Sentry group CMUXTERM-MACOS-1FEY has 1,600 events, zero users, and environment `development`.
- The representative event came from `cmux DEV.debug`, release `0.64.16 (98)`.
- Its breadcrumbs identify `/tmp/cmux-xctest-bb8486b599b6af45.sock`.
- Sentry reports the group as first seen 22 days ago and last seen 4 days ago.

Owner: macOS crash-reporting startup policy.
Invariant: automated tests must never transmit crash telemetry to the production Sentry project.
Why the old path failed: the app starts Sentry whenever the shared AppDelegate launches, including under XCTest.
Fix shape: move the startup decision behind a testable policy and bypass Sentry initialization when the process is an XCTest host.
Proof that closes it: a regression test must show XCTest process metadata disables Sentry while normal development and production app metadata still enables it.
