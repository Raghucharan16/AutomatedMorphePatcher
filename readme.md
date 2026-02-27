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
    **“This app uses code from Morphe. To learn more, visit https://github.com/MorpheApp”**  
    on your distribution page (for example, on the GitHub Pages landing page).
  - The name **“Morphe”** cannot be used for derivative works as their own product name. This repository is an **unofficial automation helper** that uses the official tools.


