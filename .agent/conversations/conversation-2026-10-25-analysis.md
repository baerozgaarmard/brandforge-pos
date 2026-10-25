# Architectural Analysis Session Log

**Date:** 2026-10-25  
**Target Repository:** `https://github.com/Raktim94/nodedr-restaurant-pos.git`  
**Conversation ID:** `69f168d2-c249-4995-b6ae-c30d51d3152a`

---

## 1. Intent
Comprehensive architectural, codebase, security, hardware, and dependency analysis of `nodedr-restaurant-pos` (OrderRestro), an open-source, offline-first Restaurant POS and management suite.

---

## 2. Key Insights & Decisions

### Core Architecture & Monorepo
- **Topology:** pnpm workspaces + Turborepo monorepo with `apps/backend` (NestJS 11 + Prisma ORM), `apps/web` (Next.js 15 App Router + React 19 + Tailwind CSS v4), and `packages/types` (shared Zod schemas and TypeScript domain contracts).
- **Relational Integrity:** PostgreSQL 16 schema with 78 models (2,268 LOC) modeling multi-tenancy (`Restaurant` -> `Branch`), immutable billing snapshots (`OrderItem`, `OrderItemModifier`), and fine-grained RBAC permissions.
- **Server-Authoritative Money:** Strict decimal precision via `round2`/`round3`/`round4`, tax-inclusive pricing standard with pro-rata discounts, and change calculations restricted to cash tender.
- **Realtime CQRS:** Socket.IO gateway with cookie-based JWT authentication, scoped rooms (`branch:<id>`, `user:<id>`), and pure server-push design where client mutations route through standard REST endpoints.
- **Model Context Protocol (MCP):** Stateless native MCP server (`/api/v1/mcp`) exposing 40 restaurant management and analytics tools to AI agents with granular RBAC permission gating.
- **Multi-Platform Packaging:**
  - Linux: Debian `.deb` packages with systemd service and Docker Compose orchestrating lean multi-stage images.
  - Windows: MSIX package hosting a C# (.NET 8) WinForms + WebView2 launcher with dual remote client and embedded server supervision modes.
  - CasaOS / ZimaOS: Preconfigured AppStore manifest with automated container updates via a zero-dependency Docker UNIX socket updater sidecar.
- **Direct Thermal Hardware Integration:** ESC/POS receipt and KOT rendering directly to Linux kernel character devices (`/dev/usb/lp*`), raw network TCP sockets (JetDirect Port 9100), and off-screen hidden iframes for browser/spooler printing.

---

## 3. High-Priority Technical Debt & Improvement Areas
1. **Sequential Order Numbering TOCTOU:** Concurrency race in `nextOrderNumber` outside transactions; UTC date slicing causes midnight rollover drift in non-UTC time zones.
2. **`OrdersService` God Class:** 2,540 lines handling order creation, billing, printing, KOTs, and refunds. Candidate for domain service decomposition.
3. **PIN Authentication Half-Implementation:** PIN login endpoint exists and is bcrypt-guarded, but no setup/management endpoint exists to write `pinHash`.
4. **AuditLog Deletion Scoping:** `AuditLog` relies on `user.restaurantId`; deleting a user unlinks their historical audit trail. Needs explicit `restaurantId` on `AuditLog`.
5. **Windows Native Thermal Printing:** USB ESC/POS printing is Linux-only; Windows relies on WebView2 print dialogs rather than raw Win32 spooler writes.

---

## 4. Files Modified / Created
- `.agent/conversations/conversation-2026-10-25-analysis.md`


---

## 5. White-Labeling & Rebranding Update (Option 1)
- **Target Brand:** BrandForge Media (`BrandForge Media POS`)
- **Frontend Changes (`apps/web`):**
  - Page title & metadata updated in `apps/web/app/layout.tsx`.
  - PWA manifest updated in `apps/web/public/manifest.json`.
  - Login & Signup forms updated in `apps/web/app/login/page.tsx` and `apps/web/app/signup/page.tsx`.
  - Brand footer & support links updated in `apps/web/components/layout/brand-footer.tsx` (pointing to `brandforgemedia.com`).
  - Fallback branding in `apps/web/components/layout/app-shell.tsx` and logo alt text in `apps/web/components/layout/logo.tsx`.
  - Settings, docs, and marketing pages updated across `apps/web/app/(dashboard)/settings/*`, `apps/web/app/docs/*`, and `apps/web/components/marketing/*`.
- **Packaging & Desktop Launcher Changes (`windows/msix`):**
  - MSIX DisplayName & PublisherDisplayName updated in `windows/msix/AppxManifest.xml`.
  - WinForms launcher titles, dialogs, error messages, and system tray tooltips updated in `MainForm.cs`, `FirstRunChoiceForm.cs`, `ServerSettingsForm.cs`, `TrayIcon.cs`, and `Launcher.csproj`.
- **Backend & Deployment Defaults:**
  - Database seed defaults in `apps/backend/prisma/seed.ts` updated to `'BrandForge Media POS (Demo)'`.
  - ESC/POS slip receipt test header in `escpos-receipt.ts` and PDF report author in `export.ts` updated to `'BrandForge Media POS'`.
  - Container names and OCI vendor labels updated in `docker-compose.yml` (`brandforge-pos-*`).
  - CasaOS manifest `author`, `developer`, `title`, and `description` updated to BrandForge Media.
  - Debian `product.conf` updated with BrandForge Media vendor & title.
- **Licensing Compliance:** Root `LICENSE` file and AGPL-3.0 copyleft terms remain intact.


---

## 6. GitHub Publication
- **Repository URL:** `https://github.com/baerozgaarmard/brandforge-pos`
- **Visibility:** Public
- **Branch:** `main`
- **Visual Assets:**
  - `docs/assets/brandforge-lockup.svg` (crisp vector lockup)
  - `docs/assets/hero-animated.svg` (animated terminal & POS ticket banner)
  - `docs/assets/install-animated.svg` (animated quickstart preview)
  - Curated genuine software screenshots embedded in `README.md` (POS Cart, KDS bump-bar, Table canvas, Analytics Dashboard, Inventory & Cash close).
- **Publication Method:** Unshallowed git tree, staged all rebranding and visual changes, authenticated via GitHub CLI, and pushed upstream tracking branch `main`.
