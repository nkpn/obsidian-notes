---
date: 2026-05-12
session_id: local_2cd2c448-2adc-45eb-9b85-d13f9f59fdc3
type: session
project: workstation-hygiene
tags: [chrome, privacy, disk-cleanup, on-device-ai, gemini-nano]
entities: [[nikita-ilin]]
tldr: Located the Gemini Nano on-device model that Chrome 147+ silently pulls (`~/Library/Application Support/Google/Chrome/OptGuideOnDeviceModel/`) plus the related ML caches — Translate Ranker, Segmentation Platform, optimization_guide_hint_cache_store. Produced a quit-Chrome → measure → `rm -rf` cleanup recipe plus the `chrome://flags` set that prevents Chrome from re-downloading the weights. Claude could not delete itself (safety policy + path outside sandbox); Nikita runs the commands.
status: complete
auto-saved: true
---

# Chrome AI Model Cleanup

## TL;DR

Chrome 147+ silently downloads multi-GB local AI model weights (Gemini Nano, OptGuideOnDeviceModel) for features most users never touch. This session located the exact paths on macOS, separated the AI/ML caches worth deleting from security-critical Chrome data that must stay, and produced a one-shot cleanup recipe. Disabling the relevant `chrome://flags` (most importantly `#optimization-guide-on-device-model`) is required after deletion — otherwise Chrome re-pulls the weights on the next idle window. The actual `rm -rf` is left for Nikita to run; Claude doesn't delete user files.

## Decisions

- **Target the on-device model directory, not Chrome cache as a whole.** Only `OptGuideOnDeviceModel/` is meaningfully large (GBs); the others are KB–MB. Savings on the smaller stores are about stopping background processing, not disk.
- **Pair deletion with the flags.** Deleting `OptGuideOnDeviceModel/` alone won't stick — Chrome will re-download on idle. The cleanup is delete + disable flags + relaunch, in that order, with a full ⌘Q quit before the delete.
- **Touch only AI/ML caches.** Explicitly leave `PKIMetadata/`, `CertificateRevocation/`, `SafetyTips/`, `MEIPreload/`, `ZxcvbnData/` alone — those are security-critical or trivially small. Cleanup list is bounded: `OptGuideOnDeviceModel/`, `Segmentation Platform/`, `optimization_guide_hint_cache_store/`, and the two `Translate Ranker Model` files (Default + Profile 1).
- **Claude does not run the deletion.** Safety policy + the paths sit in `~/Library/...` which is outside the bash sandbox. Recipe handed off as paste-ready Terminal commands.

## Deliverables

- Verified path on this machine: `~/Library/Application Support/Google/Chrome/OptGuideOnDeviceModel/2025.8.8.1141/` with `weights.bin`, `encoder_cache.bin`, `adapter_cache.bin`, `on_device_model_execution_config.pb`, `manifest.json`, `_metadata/verified_contents.json`.
- Two-step Terminal recipe: `du -sh` over the five targets to measure first, then `rm -rf` over the same paths.
- Confirmed flag list from the original user message — most important: `#optimization-guide-on-device-model`, plus the `#prompt-api-*`, `#writer-api-*`, `#rewriter-api-*`, `#proofreader-api-*`, `#summarizer-api-*` family, `#omnibox-ml-url-scoring*`, `#contextual-tasks*`, and `#page-content-annotations*`.

## Open threads

- Sizes not measured (sandbox can't reach `~/Library`). Nikita runs the `du -sh` step to see the actual reclaim before deleting.
- No verification step after relaunch — worth re-running `ls ~/Library/Application\ Support/Google/Chrome/OptGuideOnDeviceModel/` after a day of normal browsing to confirm Chrome didn't re-pull. If it did, a flag was missed.
- Same hygiene pass not yet done for Chrome Beta/Canary or other Chromium-based browsers (Edge, Brave, Arc) — they share the OptGuide service.

## Linked entities

[[nikita-ilin]]
