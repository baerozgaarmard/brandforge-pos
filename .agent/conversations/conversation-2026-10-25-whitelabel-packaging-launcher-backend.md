# Conversation Log: White-Label Packaging, Desktop Launcher, and Backend Defaults

**Date:** 2026-10-25
**Task:** Rebrand packaging manifests, desktop client metadata, and backend seed defaults to "BrandForge Media" / "BrandForge POS".

## Intent
White-label the packaging manifests, desktop client launcher metadata, and backend defaults across the repository to the BrandForge Media brand identity while strictly preserving all functional logic, port numbers, database columns, env var names, container network wiring, and AGPL-3.0 licensing.

## Key Decisions Made
1. **Windows MSIX Manifest (`windows/msix/AppxManifest.xml`)**:
   - Updated `DisplayName` in Properties and VisualElements to `"BrandForge Media POS"`.
   - Updated `PublisherDisplayName` to `"BrandForge Media"`.
   - Updated `Description` in VisualElements to `"BrandForge Media Restaurant POS"`.
   - Kept package Identity Name and Publisher CN unchanged to preserve Windows Partner Center Store identity validation.

2. **Windows Desktop Launcher (`windows/msix/Launcher/`)**:
   - `MainForm.cs`: Updated main window title, fatal error headline, dialog titles, and notification balloons to `"BrandForge Media POS"`.
   - `FirstRunChoiceForm.cs` & `ServerSettingsForm.cs`: Updated setup & connection window titles and labels to `"BrandForge Media POS"`.
   - `TrayIcon.cs`: Updated tray icon context menu items and tooltip text to `"BrandForge Media POS"`.
   - `Launcher.csproj`: Updated `<Product>` to `"BrandForge Media POS"` and `<Company>` to `"BrandForge Media"`.
   - Preserved AppConfig paths (`%LOCALAPPDATA%\OrderRestro`) and internal namespaces to ensure backward compatibility and prevent data disconnection.

3. **CasaOS Packaging (`casaos/docker-compose.yml`)**:
   - Updated title to `"BrandForge Media POS"`.
   - Updated author and developer metadata to `"BrandForge Media"`.
   - Updated web port description and application description to BrandForge Media POS.
   - Preserved all functional ports, mount points, database credentials, and container names.

4. **Debian Packaging (`packaging/deb/product.conf`)**:
   - Updated `TITLE` to `'BrandForge Media POS'`.
   - Added `VENDOR='BrandForge Media'`.
   - Updated `SHORTDESC` and `LONGDESC` to reflect BrandForge Media POS while preserving package name (`nodedr-orderrestro`), ports, and path configuration.

5. **Backend Database Seed & Receipt (`apps/backend/prisma/seed.ts` & orders module)**:
   - `apps/backend/prisma/seed.ts`: Updated demo restaurant name to `'BrandForge Media POS (Demo)'` and branch name to `'BrandForge Media POS - Main Branch'` in both create and update upserts.
   - `apps/backend/src/modules/orders/escpos-receipt.ts`: Updated ESC/POS test slip receipt header to `'BrandForge Media POS'`.
   - `apps/backend/src/modules/reports/export.ts` & `report-schedules.service.ts`: Updated report author and email signature to `"BrandForge Media POS"`.
   - `apps/backend/src/main.ts`: Updated Swagger API title/description and startup console message to `"BrandForge Media POS API"`.
   - `apps/backend/package.json`: Updated description to `"BrandForge Media POS API"`.

6. **Downloads Service (`apps/backend/src/modules/downloads/downloads.service.ts`)**:
   - Added BrandForge Media POS download channels documentation header while keeping verified download channels and security whitelist safe and functional.

7. **Docker Compose (`docker-compose.yml` & `install.sh`)**:
   - Updated container names to `brandforge-pos-postgres`, `brandforge-pos-backend`, `brandforge-pos-web`, `brandforge-pos-updater`.
   - Added OpenContainer image labels identifying Title and Vendor as `"BrandForge Media"`.
   - Updated `TARGET_CONTAINERS` to support both `brandforge-pos-*` and legacy `nodedr-restaurant-*` targets for zero-downtime upgrades.
   - Updated `install.sh` demo seed command to check for `brandforge-pos-backend` with fallback.
   - Kept all ports, database passwords, env var names, and network wiring strictly intact.

## Files Modified
- `windows/msix/AppxManifest.xml`
- `windows/msix/Launcher/MainForm.cs`
- `windows/msix/Launcher/FirstRunChoiceForm.cs`
- `windows/msix/Launcher/ServerSettingsForm.cs`
- `windows/msix/Launcher/TrayIcon.cs`
- `windows/msix/Launcher/Launcher.csproj`
- `casaos/docker-compose.yml`
- `packaging/deb/product.conf`
- `apps/backend/prisma/seed.ts`
- `apps/backend/src/modules/orders/escpos-receipt.ts`
- `apps/backend/src/modules/reports/export.ts`
- `apps/backend/src/modules/reports/report-schedules.service.ts`
- `apps/backend/src/main.ts`
- `apps/backend/package.json`
- `apps/backend/src/modules/downloads/downloads.service.ts`
- `docker-compose.yml`
- `install.sh`

## Next Steps
- Verify build or container launch in test environment if needed.
- No further action required for packaging rebranding.
