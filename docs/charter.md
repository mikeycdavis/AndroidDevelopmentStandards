<!-- Charter revision 3; authoritative evidence: F:\GitHub Evidence\AndroidDevelopmentStandards\evidence-1\ -->
# Charter packet 04 — AndroidDevelopmentStandards

Status: DRAFT, read-only. Backlog IDs are draft labels, not issue numbers. See
`00-shared-conventions.md` (kept in the evidence folder above, not in this repository). Scope below follows Mike's list (on or before 2026-11-19 19:36 PST (2026-11-20T03:36Z)).

## 1. Purpose

Govern Android development and Google Play delivery: signing, credentials, platform security,
privacy, build integrity, testing evidence, and release operations.

> A clean result means: for the Android surfaces this project declared and the framework could
> actually evaluate from project files and ingested evidence, the recorded evidence supports it —
> and everything requiring a device, Play Console, or a human remains visible as unevaluated.

## 2. Scope (mapped from Mike's list)

| Mike's item | Proposed standard(s) |
| --- | --- |
| Play Console roles, least privilege, API access, service-account governance | 04, 05 |
| Play App Signing, upload-key custody, rotation/reset, certificate fingerprints | 06 |
| No keystores, passwords, API keys, service-account JSON, signing materials in source or evidence | 07 |
| Android Keystore, encrypted local storage, backup exclusions, device-bound secrets | 10 |
| Manifest permissions, runtime permissions, exported components, intents, deep links, App Links | 08, 09, 11 |
| Network Security Configuration, HTTPS-only defaults, debug trust anchors, reviewed exceptions | 12 |
| Gradle/AGP/Kotlin/Java/SDK/NDK/dependency pinning | 17 |
| Dependency provenance, vulnerability scanning, SBOMs, repository restrictions, reproducibility | 18 |
| Build variants; dev/test/staging/prod separation | 19 |
| R8/ProGuard, release-build testing, logging removal, debuggability, tamper considerations | 20 |
| Data Safety declarations, privacy behaviour, SDK data collection, retention, deletion, consent | 13–16 |
| Testing: unit, instrumentation, Compose/UI, accessibility, screenshot, performance, startup, ANR, crash, offline, upgrade, migration | 25–27 (verification procedure cites SoftwareTestingStandards) |
| Device/form-factor/Android-version/API-level/locale/orientation/config-change matrices | 28 |
| Internal/closed/open/production tracks; staged rollout, halt, rollback, post-release monitoring | 22–24 |
| `versionCode`, `versionName`, artifact provenance, `.aab` verification, release evidence | 21 |

## 3. Non-goals

- **No access to Play Console, Google Cloud, keystores, or any credential.** Evidence is key
  fingerprints (SHA-256 of certificates), key IDs, role lists, timestamps and hashes — never key
  material, passwords, or service-account JSON.
- Does not create, rotate, reset or request any key, track, or setting. Rotation/reset is governed
  as a *procedure and evidence*, never performed.
- Does not require Android Studio/emulator/SDK in the evaluator; runs on Node (Windows-friendly).
  Build, `apksigner`/`bundletool` verification, lint and test results are ingested or attested.
- Does not own UI/interaction/accessibility design rules (UIUXDesignStandards) or generic test
  integrity (EngineeringStandards 47) or universal invariants (secrets, release integrity, SCM,
  dependency-management principles — EngineeringStandards). Android rules defer to those and add the
  Android mechanism.
- Not a Play policy lawyer: does not predict review/enforcement outcomes or interpret Play Developer
  Program Policy; checks documented readiness and declaration consistency.
- Cross-platform frameworks governed for their Android-native surface only in v1; Wear OS, Android
  TV/Auto, and ChromeOS deferred unless a consumer needs them.

## 4. Authoritative first-party sources (all UNVERIFIED until opened)

> Status note: this section is the pre-verification charter text. The source ledger in the evidence folder (hashed in `docs/evidence-index.md`) records which sources were later opened (tool-extracted, not human-reviewed) and governs; "unverified" below is superseded per source by the ledger.

Cited by Mike (taken as his citation, not yet opened by me): Android security tips/checklist
(`developer.android.com/privacy-and-security/security-tips`), Network Security Configuration
(`.../security-config`), Play App Signing (`support.google.com/googleplay/android-developer/answer/9842756`),
core app-quality guidance (`developer.android.com/docs/quality-guidelines/core-app-quality`).

Additional to open and record:

| Source | Role |
| --- | --- |
| Android Developers: app manifest reference, permissions, app links verification, intents | Manifest/IPC controls |
| Android Keystore system, Jetpack Security / data-at-rest and backup docs | Key and storage controls |
| Android Gradle Plugin release notes, SDK/target-API requirements, R8 shrinker docs | Build controls |
| Play Console Help: roles/permissions, API access and service accounts, release tracks, staged rollouts, app bundles | Access and release |
| Google Play Developer Program Policy; Data safety section help; target-API-level requirement | Policy and privacy declarations (dated; changes often) |
| Google Play Developer API / Publishing API reference | Release evidence fields |
| Android Vitals / Play Console quality metrics documentation (ANR, crash thresholds) | Post-release monitoring |
| Android testing documentation (local, instrumented, Compose, screenshot, Macrobenchmark) | Inputs to the Testing pack, not copied |
| Android security bulletins; Android Compatibility Definition | Currency and compatibility |
| Internal: EngineeringStandards 16, 46, 49, 50; SoftwareTestingStandards; UIUX ADR 0006 pattern | Boundary partners |

## 5. Proposed hierarchy

Layout per conventions §2. Proposed ~32 standards:

| # | Standard | Group |
| --- | --- | --- |
| 01 | Boundary with EngineeringStandards, UIUXDesignStandards, SoftwareTesting, AppleDevelopment | Foundations |
| 02 | Android applicability (Gradle/AGP, `AndroidManifest.xml`) and declared targets | Foundations |
| 03 | Evidence surfaces: manifest/Gradle static, ingested tool output, attested | Foundations |
| 04 | Play Console roles and least privilege | Access |
| 05 | Play API access and service-account governance | Access |
| 06 | Play App Signing, upload-key custody, rotation/reset, fingerprints | Signing |
| 07 | No signing material or credentials in source or evidence | Signing |
| 08 | Manifest permissions and runtime permission requests | Platform security |
| 09 | Exported components and intent handling | Platform security |
| 10 | Android Keystore, encrypted storage, backup exclusions | Platform security |
| 11 | Deep links and verified App Links | Platform security |
| 12 | Network Security Configuration and HTTPS-only | Platform security |
| 13 | Data Safety declaration accuracy | Privacy |
| 14 | SDK data collection inventory | Privacy |
| 15 | Retention, deletion and consent behaviour | Privacy |
| 16 | Logging, analytics and identifiers hygiene | Privacy |
| 17 | Toolchain pinning: Gradle, AGP, Kotlin, Java, SDK, NDK | Build |
| 18 | Dependency provenance, repository restrictions, scanning, SBOM, reproducibility | Build |
| 19 | Build variants and environment separation | Build |
| 20 | R8/ProGuard, release-build testing, logging removal, debuggability, tamper statement | Build |
| 21 | `versionCode`/`versionName`, `.aab` verification, artifact provenance | Release |
| 22 | Release tracks: internal, closed, open, production | Release |
| 23 | Staged rollout, halt and rollback | Release |
| 24 | Post-release monitoring (Vitals, crash, ANR) | Release |
| 25 | Unit, instrumentation, Compose/UI test evidence | Verification |
| 26 | Accessibility, screenshot, performance, startup, ANR and crash test evidence | Verification |
| 27 | Offline, upgrade and migration test evidence | Verification |
| 28 | Device, API-level, form-factor, locale, orientation, config-change matrix | Compatibility |
| 29 | Target-API and policy currency review cadence | Currency |
| 30 | AI-agent operation on Play/Google accounts: propose-vs-execute, no custody | AI |
| 31 | Policy, applicability, exceptions | Framework |
| 32 | Evidence, assurance, compliance output | Framework |

Rule-ID domains (proposed): `andaccess`, `andsign`, `andsec`, `andnet`, `andprivacy`, `andbuild`,
`andrelease`, `andcompat`.

Applicability: `build.gradle(.kts)` with the Android plugin, `AndroidManifest.xml`, `settings.gradle`
with Android modules. Declared `no-android` is a declaration, not a finding.

## 6. Initial threat model

| ID | Failure | Detection posture |
| --- | --- | --- |
| T1 | Keystore, `keystore.properties`, passwords, service-account JSON, API keys committed or placed in evidence | Static scan (file types, PEM/JSON key patterns, Gradle signingConfig literals); never echoes values |
| T2 | Release signed with a key whose custody is one person/laptop; no upload-key reset plan | Attested custody + fingerprint ledger |
| T3 | Upload key and app-signing key conflated, or Play App Signing not enrolled | Attestation + fingerprint consistency |
| T4 | Over-broad service account / Play roles; shared credentials in CI | Role-list attestation; CI config static checks |
| T5 | Exported component without protection; implicit-intent exposure; unverified deep links | Manifest static analysis `partial` |
| T6 | Cleartext traffic permitted; debug trust anchors or user CAs in release | NSC + manifest static check; release-vs-debug scoping |
| T7 | Sensitive data in backups or plaintext local storage | `allowBackup`/backup rules check; storage patterns `partial` |
| T8 | Dependency from unrestricted repository / unpinned / vulnerable | Gradle resolution and repository checks; scanner output ingested |
| T9 | Debug config, logging or test endpoints in release variant; R8 disabled | Variant configuration check; release-build attestation |
| T10 | Data Safety form disagrees with real SDK/data behaviour | SDK inventory vs declaration; behaviour attested |
| T11 | Release artifact not traceable to commit (`versionCode` reuse, `.aab` mismatch) | Provenance record check |
| T12 | Full-production release with no staged rollout, no halt plan, no monitoring | Release-plan attestation |
| T13 | Tests pass only on one device/API level; upgrade path untested | Matrix evidence; missing cells shown as not-evaluated |
| T14 | Evaluator implies device, store or signature verification it never performed | Charter sentence; those surfaces not-evaluated by default |

## 7. First backlog (draft labels; 15 items)

| Draft ID | Title | Notes |
| --- | --- | --- |
| AND-B01 | Repo skeleton, vendored core provenance, rule-ID domains | |
| AND-B02 | Boundary standard vs Eng / UIUX / Testing / Apple | Encodes the 4-line ownership rule; meta-test |
| AND-B03 | Open and record external sources (Mike's four plus the list above), dated review cadence | Resolves UNVERIFIED |
| AND-B04 | Android applicability classifier (Gradle/manifest) | |
| AND-B05 | Policy schema, template, declared targets/variants | |
| AND-B06 | Pin a version of the SoftwareTestingStandards core release-evidence schema; define the Android profile (keystores, Play App Signing, tracks, Play Console fields) and ingestion of `apksigner`/`bundletool`/lint/test outputs | Never executes tools; no duplicated core schema; depends on SWT-B04 |
| AND-B07 | Signing and credential standards 06–07 + secret-file/signingConfig detectors | Output-safety test |
| AND-B08 | Manifest parser and platform-security standards 08–11 | Zero-dependency XML parsing |
| AND-B09 | Network Security Configuration parser and standard 12 | Release-vs-debug scoping |
| AND-B10 | Build standards 17–20 (Gradle/version catalog/repositories/R8) | |
| AND-B11 | Privacy standards 13–16 with SDK inventory model | |
| AND-B12 | Release standards 21–24 + attestation model for tracks/rollout | |
| AND-B13 | Fixtures: compliant, keystore-in-repo, cleartext allowed, exported-unprotected, unpinned deps | |
| AND-B14 | StandardsEnforcer adapter entry | Cross-repo |
| AND-B15 | Adversarial fixtures proving each rule class; real-project adoption recorded as a later milestone | No qualifying real Android workload under F:\Repos; no adoption evidence invented |

## 8. Applied decisions and open questions

Applied (Mike, on or before 2026-11-19 19:36 PST (2026-11-20T03:36Z)): v1 validated by adversarial fixtures only (no qualifying real workload under F:\Repos); pins the SoftwareTestingStandards core release-evidence schema and extends it with a platform profile.


1. ~~Real Android project to validate against?~~ Settled by the applied decision above: fixture-only validation.
2. Kotlin/Compose only, or also Java/XML views and NDK in v1? Recommendation: Kotlin/Gradle
   first; NDK as pinning rule only.
3. ~~Shared mobile release-evidence design.~~ Settled by the applied decision above: SoftwareTestingStandards owns the core release-evidence schema; Android pins an exact core version and extends only its own namespace. The core remains unfrozen (see AND-B06).
