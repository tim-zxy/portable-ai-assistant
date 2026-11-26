# 13 --- Release, External Delivery and Enterprise Handoff

## 1. Phase 1 release products

-   Windows x64 Installer.
-   Portable ZIP.
-   Web UI served by the local assistant.
-   Desktop application.
-   CLI. 
-   Complete source package and Git history.
-   Offline dependency/build materials.
-   Official extensions and Skills.
-   Release manifest, hashes, SBOM and verification evidence.

OpenCode and DeepSeek Harness are normally discovered/reused or
installed separately. Do not bundle model weights or user credentials.

## 2. Versioning

Use SemVer for the product and independent versions for extensions. A
release manifest records the exact tested combination.

Development builds may use internal identifiers. Formal releases use
stable tags.

## 3. Package/data separation

Program files and user data must be separate. Uninstalling the program
must not silently delete user workspaces, memory, artifacts or
credentials.

Offer explicit data cleanup with a clear impact summary.

## 4. Release integrity

Provide:

-   Source commit and tag.
-   Build environment/tool versions.
-   Artifact SHA-256 manifest.
-   CycloneDX SBOM.
-   Build provenance.
-   Windows Authenticode signature for formal Windows distribution.
-   Tauri updater signature if the updater is enabled.
-   Test and acceptance reports.
-   License inventory.

Authenticode and Tauri updater signing are different trust mechanisms.
Protect formal signing keys outside developer workstations and external
Codex sessions.

## 5. Offline delivery

Prepare a reproducible offline build/install path for the internal
environment:

-   Locked dependencies.
-   Approved offline packages/cache.
-   Native runtime dependencies.
-   WebView2 online/offline installation option.
-   Extension packages.
-   License and SBOM data.
-   Hash and signature verification.
-   No hidden public network downloads during offline installation.

PAA offline installation does not imply public model/Feishu/GitHub
services are available offline. Those integrations must degrade
explicitly.

## 6. External versus internal responsibilities

External development delivers a complete general product and public
extension contracts.

Internal development adds company-specific:

-   SCM/CI.
-   IM.
-   Knowledge systems.
-   Model gateway.
-   Identity and Vault.
-   Secure Worker and cloud deployment.

Do not embed enterprise-specific business logic into the generic core.

## 7. Suggested repository separation

-   `portable-ai-assistant`: core, official extensions, public contracts
    and release.
-   `portable-ai-assistant-enterprise`: internal adapters, internal
    configuration and deployment.

The enterprise repository pins the upstream core version. Internal
patches must be explicit and traceable; do not claim an unchanged
upstream version if core code has diverged.

## 8. Internal acceptance

1.  Register incoming release.
2.  Verify signatures and hashes.
3.  Review SBOM and licenses.
4.  Import source and preserve provenance.
5.  Build inside the internal network.
6.  Add internal credentials and extensions.
7.  Run actual internal Agent/model/SCM/CI workflows.
8.  Publish a versioned enterprise combination.

## 9. Backup and migration

Backup a consistent database snapshot and referenced artifacts. Default
backup excludes secret material and decryption keys.

On migration to another device or cloud:

-   Restore business state and memory.
-   Re-discover Agents/models.
-   Rebind credentials.
-   Rebind repositories and workspaces.
-   Reconcile external operations.
-   Ensure only one authoritative control plane resumes dispatch.

## 10. Update channels

Stable, Beta and Internal. Offline environments use signed offline
update packages and internal distribution.

## 11. Release ownership

Assign clear responsibility for Product, Core, Runtime, Integration,
Security, Evaluation and Release. One person may hold multiple
responsibilities in Phase 1, but each responsibility must be assigned.
