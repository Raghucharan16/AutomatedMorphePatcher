## Automated Morphe Patcher

This repository is intended to **automate patching of APKs for YouTube, YouTube Music, and Reddit using Morphe CLI and Morphe Patches**, and then **publish the patched APKs via GitHub Pages**.

### What this repo will do

- **Download Morphe CLI and Morphe Patches bundle** from their latest GitHub releases.
- **Download source APKs** (for YouTube, YouTube Music, and Reddit) from URLs you provide (for example, direct APKMirror download links).
- **Apply the latest Morphe patches** to each APK using Morphe CLI.
- **Export patched APKs and a simple `index.html`** into a `dist/` folder.
- **Deploy `dist/` to GitHub Pages** via a GitHub Actions workflow.

> **Important:** APKMirror is behind anti‑bot protections and does not provide a public API.  
> In practice, this means a fully reliable “auto‑discover latest APK from APKMirror” step cannot be guaranteed from GitHub Actions alone.  
> The workflow will be designed to take **direct APK download URLs** (which can be from APKMirror or elsewhere). You can update those URLs (or automate that part yourself) to always point at the latest supported versions.

### High‑level architecture

- `config/apps.yml`: configuration for each app (package name, human name, APK download URL, output filename, etc.).
- `scripts/patch_apps.py`: Python script that:
  - fetches latest Morphe CLI JAR,
  - fetches latest Morphe patches `.mpp`,
  - downloads each configured APK,
  - runs `java -jar morphe-cli.jar patch --patches patches.mpp ...`,
  - writes patched APKs and a simple listing page into `dist/`.
- `.github/workflows/patch-and-deploy.yml`: GitHub Actions workflow that:
  - runs on `workflow_dispatch` and optional `schedule`,
  - executes `scripts/patch_apps.py`,
  - publishes `dist/` via GitHub Pages.

### Legal and licensing notes

- This project uses **Morphe CLI** and **Morphe Patches**:
  - Morphe CLI: `https://github.com/MorpheApp/morphe-cli`
  - Morphe Patches: `https://github.com/MorpheApp/morphe-patches`
- Morphe Patches are licensed under **GPLv3 with additional Section 7 conditions**. In particular:
  - You must show the attribution text:  
    **“This app uses code from Morphe. To learn more, visit https://morphe.software”**  
    on the distribution page (for example, on the GitHub Pages landing page).
  - The name **“Morphe”** cannot be used for derivative works as their own product name. This repository is an **unofficial automation helper** that uses the official tools.

Make sure you are allowed to:

- Download and redistribute the original APKs you patch.
- Distribute the patched APKs in your jurisdiction and under the app publisher’s terms.

### How to configure APK sources

- **Edit `config/apps.yml`** and, for each app, set:
  - `apk_url`: a direct download link to the APK/APKM of the version you want to patch (commonly from APKMirror).
  - `output_file`: name of the patched file that will appear on the GitHub Pages site.
- The script does **no scraping** of APKMirror; it only downloads from the URLs you provide.
  - You can manually update these URLs whenever Morphe adds support for a newer version.
  - Or, you can maintain a small separate script or workflow that updates `config/apps.yml` to always point to the latest supported APK URLs.

### Running the automation

1. Push this repository to GitHub and enable **GitHub Pages** (Source: “GitHub Actions”).
2. In GitHub, go to **Actions → Automated Morphe Patcher → Run workflow** to trigger patching on demand, or let the cron schedule run.
3. When the workflow finishes, the **GitHub Pages URL** will serve `dist/index.html` with download links to the patched APKs.


