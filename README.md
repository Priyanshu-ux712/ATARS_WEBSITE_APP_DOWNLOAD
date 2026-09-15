# ATARS 5.4.6 — GitHub release assets

Everything in this folder, and nothing else, goes on the **v5.4.6** release at
`github.com/Priyanshu-ux712/ATARS_WEBSITE_APP_DOWNLOAD/releases`.

| File | What it is |
|---|---|
| `ATARS-Setup.exe` | The download the website and the in-app updater both point at. Per-user Inno Setup installer, no admin rights. |
| `LICENSE.txt` | Product licence. Must travel with the installer. |
| `EULA.txt` | The agreement the installer shows and the user accepts. Must travel with the installer. |
| `THIRD-PARTY-NOTICES.txt` | Attribution for the open-source components bundled inside the app. Required by their licences. |
| `SHA256SUMS.txt` | The installer checksum, generated from the file in this folder. The security page tells users to compare against it. |

```
2b600ebba5718ae5d1e0019a34c07fc69d5a90dc7f6a659346b732018bc5a2c5  ATARS-Setup.exe
```

Upload `ATARS-Setup.exe` **first** — the site and the in-app updater read
`releases/latest/download/ATARS-Setup.exe`, and a release announcing a version whose file
is not there yet makes every installed copy fail its update check.

The installer is **not code-signed**, so SmartScreen will warn until a certificate is in
place. That is expected, it is documented on the site’s security page, and the checksum
above is what a cautious user compares against.

The Microsoft Store package (`ATARS.msix`) and its `ATARS.appinstaller` feed are **not**
here — they go to Partner Center, and they live one level up in `RELEASE-5.4.6/`.
