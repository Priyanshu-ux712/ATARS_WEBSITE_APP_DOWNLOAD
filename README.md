# ATARS 5.4.7 — everything to ship, in one folder

Built 2026-09-20, 12:07 → 14:29 (2 h 22 m). Two artifacts, two places they go.
Nothing here has been uploaded yet.

The large files are **hardlinks**, not copies — they cost no extra disk, and they are
the same bytes as the build output in `dist\`. Do not rename them: the filenames are
what the download links expect.

Unlike 5.4.6, this is an **update**, not a first listing. The Store entry, the art and
the screenshots are already live; only the package and the "What's new" text change.

---

## 1 · `ATARS-Setup.exe` — the website download

**350.4 MB.** Inno Setup installer, per-user, no admin rights needed.

```
sha256  1871ec7b4d2eb6c9bb44f80dd756cb8bf1374786b71bd7131eee2bb2c2b8816b
```

**Where it goes:** GitHub Releases, tag `v5.4.7`, in
`Priyanshu-ux712/ATARS_WEBSITE_APP_DOWNLOAD`. The release body is ready in
`github-release/RELEASE-NOTES.md` — paste it as the description.

**Upload this FIRST**, before the site goes out. `site/latest.json` and the in-app
updater both read `releases/latest/download/ATARS-Setup.exe`, which is a *floating*
URL: announce 5.4.7 while that URL still serves the 5.4.6 file and every installed
copy downloads the old installer and fails its checksum. The manifest is already
built and signed; it is inert until the site is deployed.

Not code-signed — SmartScreen will warn until a certificate is bought. Expected, not
a defect, and the site says so next to the button.

---

## 2 · `ATARS.msix` — the Microsoft Store

**506.3 MB**, **unsigned on purpose** — Microsoft signs it. Do not sign it.

```
sha256  1aeda4511097f62c8242e0ec295e84dc0c5d83fde548750ca36497ba952b240c
```

Identity, read back out of the built package:

```
Name                  PriyanshuKumar.ATARS
Publisher             CN=C0F0C134-91E6-4CF3-AC8F-AF5ECE21AAB9
PublisherDisplayName  Priyanshu Kumar
Version               5.4.7.0   x64
```

**Where it goes:** Partner Center → ATARS (`9NFRBPL4T2M8`) → new submission →
Packages. Nothing else in the listing needs to change. Add a "What's new" line —
`github-release/WHATS-NEW.txt` has one sized for the Store.

### The trap that already caught this release once

Partner Center **carries forward the packages from the live submission**, so a fresh
draft arrives already holding 5.4.6. On 20 Sept the draft showed two entries, both
`v5.4.6.0`. Remove **every** carried-forward entry before uploading, or the
submission either ships 5.4.6 again or is rejected for not raising the version.

There are two MSIX files on this machine and they are one megabyte apart:

| Path | Version | Size |
|---|---|---|
| `RELEASE-5.4.6\ATARS.msix` | 5.4.6.0 | 505.5 MB |
| `RELEASE-5.4.7\ATARS.msix` (this folder) | **5.4.7.0** | **506.3 MB** |

Size is a weak check. Read the version Partner Center displays after upload — it must
say **v5.4.7.0**.

`ATARS.appinstaller` is only for *sideloading* — the feed a sideloaded MSIX polls for
updates. The Store does not use it.

---

## The order that matters

1. `ATARS-Setup.exe` → GitHub Releases, tag `v5.4.7`
2. **then** deploy `site/` to Netlify — `latest.json` is the update switch
3. `ATARS.msix` → Partner Center (independent of 1 and 2)
4. Commit `site/latest.json` + `site/index.html`, merge `feat/workbook` → `main`

Steps 1 and 2 are order-dependent. Step 3 is not.

The terminal edition is unchanged at **v0.1.0** — nothing to publish to PyPI, and
`tools/release.py sync` must not be run, because it rewrites that `v0.1.0` on the site
to the app's version.

---

## What was verified before this folder existed

Checked against the artifacts, not the build log:

| Check | Result |
|---|---|
| `WARNING: shipping PLAIN SOURCE` | absent — the compiled `.pyd` modules shipped |
| `zxingcpp.cp311-win_amd64.pyd` in `_internal` | present, 1.83 MB — photo verification works |
| `atars_app` / `atars_auth` / `atars_billing` `.pyd` | all three compiled by Nuitka |
| `pytest tests/desktop` | **64 passed**, no skips |
| `pytest tests/engine/test_certificate_verify.py` | **26 passed**, no skips |
| App launch | own window, "ATARS — AI Analytics", 57 s to first paint |
| Shutdown | closes on ✕, ~30 s teardown, no orphaned process |
| `python tools/release.py check` | `signature VERIFIES` |

The app reports *"no file window available (tkinter is missing)"* on the home screen.
That is deliberate — `tkinter` is in `excludes` at `desktop/atars.spec:171`, and the
app falls back to the Upload control and typed paths.

---

## Build record

Nuitka 52 m · PyInstaller 57 m · Inno 17 m · MSIX 11 m.

Nuitka beat its 1 h 40 m baseline because `ccache` still held 4 of the 5 C object
files from the run interrupted on 20 Sept — **an interrupted Nuitka run is not
wasted; do not clear `build/compiled` before retrying.**

`cc1.exe` peaked at **6–8.4 GB** on `module.atars_app.c`, far above the 1.7–2.3 GB
seen on 5 Sept. It survived only because the page file is system-managed with 137 GB
of free disk, so the commit limit grew past 26 GB. When judging a future build, watch
**commit headroom** (RAM-free + pagefile-free), not free RAM — free RAM sits at zero
for that whole stage and means nothing.
