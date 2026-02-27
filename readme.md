## Automated Morphe Patcher

This repository **automates patching of YouTube, YouTube Music, and Reddit APKs using Morphe CLI and Morphe Patches**, and publishes the patched builds as **GitHub Releases**. A separate static `index.html` can be hosted (e.g. via GitHub Pages) to point users to the Releases page without exposing direct APK links.

### How it works

- **Every 2 hours** (and on manual dispatch), the workflow in `.github/workflows/patch-and-deploy.yml`:
  - Queries `MorpheApp/morphe-patches` for the **latest release tag**.
  - Checks if this repository already has a **Release with that tag**.
  - **If a Release for that tag already exists** → it **does nothing** (no APK downloads, no patching).
  - **If no Release exists yet for that tag** → it runs `src/build/morphe.sh all` once:
    - `src/build/utils.sh` downloads latest **Morphe CLI** and **Morphe Patches**.
    - Determines compatible app versions via the Morphe CLI `list-patches` command.
    - Downloads the appropriate APKs from **APKMirror** and merges splits if needed.
    - Applies the Morphe patches for:
      - YouTube
      - YouTube Music
      - Reddit
    - Writes the patched APKs into the `release/` directory.
  - Creates a **GitHub Release** tagged with the Morphe patches version and attaches all `release/*.apk`.

### Where users download from

- End‑users should download builds **only from the Releases page** of this repository.
- A static `index.html` (at the repo root) can be hosted on GitHub Pages and will:
  - Show a simple landing page.
  - Provide a single button/link that redirects users to the **Releases** tab.
  - Remind users that **MicroG (or equivalent) is required** for certain apps (e.g. YouTube / YouTube Music) to function properly.

### Legal and licensing notes

- This project uses **Morphe CLI** and **Morphe Patches**:
  - Morphe CLI: `https://github.com/MorpheApp/morphe-cli`
  - Morphe Patches: `https://github.com/MorpheApp/morphe-patches`
- Morphe Patches are licensed under **GPLv3 with additional Section 7 conditions**. In particular:
  - You must show the attribution text:  
    **“This app uses code from Morphe. To learn more, visit https://morphe.software”**  
    on your distribution page (for example, on the GitHub Pages landing page).
  - The name **“Morphe”** cannot be used for derivative works as their own product name. This repository is an **unofficial automation helper** that uses the official tools.

Please ensure you are allowed to:

- Download and redistribute the original APKs you patch.
- Distribute the patched APKs in your jurisdiction and under the original app publishers’ terms.

### Running the automation

1. Push this repository to GitHub.
2. Ensure **Actions** are enabled.
3. Optionally adjust the cron in `.github/workflows/patch-and-deploy.yml` (defaults to every 2 hours).
4. Use **Actions → Automated Morphe Builder → Run workflow** to test once.
5. Check the **Releases** tab; a Release should appear with the latest Morphe patches tag and the patched APKs.

To host the static landing page:

1. Keep `index.html` at the repository root.
2. In GitHub **Settings → Pages**, set the source to the branch containing `index.html`.
3. Share the GitHub Pages URL with users; it will send them to the Releases page instead of directly linking APKs.