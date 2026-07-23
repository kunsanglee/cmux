Symptom: Embedded iPhone and iPad interaction feels slightly laggier than Simulator.app.
User impact: Pointer input in the native Simulator pane has perceptibly slower visual feedback than Apple’s reference app.
Source: User report after PR 7857 dogfood.
Target surface: macOS native Simulator pane and isolated Simulator worker.
Build/version/tag: feat-simulator-pane at d943b9d98c, pending baseline tag.
Repro workload: Warm repeated taps and drags on the same booted iPad, comparing the embedded pane with Simulator.app.
Expected bad behavior: Input-to-pixel feedback remains measurably slower in the embedded pane after the existing 16 ms interactive publication deadline.

Primary class: UI interaction latency.
Evidence plan: Measure HID dispatch, first post-input framebuffer callback, worker publication, host copy, and presentation on one warm workload; use Time Profiler only where runtime attribution is still ambiguous.
