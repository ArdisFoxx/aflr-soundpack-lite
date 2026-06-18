# Auto-deploy setup for AFLP Soundpack (Lite)

These three files mirror the release pipeline used by ArdisFoxx's Lewd PF2e.

## Files

- `.github/workflows/main.yml` - the release workflow. Drop in as-is.
- `module.json` - a template. See "Reconciling module.json" below.
- `.gitattributes` - marks audio as binary so git leaves it untouched.

## Reconciling module.json

The workflow fills four fields at release time. Whichever module.json you keep,
these four MUST contain the exact token placeholders:

    "version":  "#{VERSION}#",
    "url":      "#{URL}#",
    "manifest": "#{MANIFEST}#",
    "download": "#{DOWNLOAD}#"

If you already have a working module.json for the soundpack, do NOT replace it.
Just change those four fields to the tokens above and keep the rest of yours
(your real id, flags, audio metadata, etc). The provided module.json is only a
starting point if you are creating the manifest fresh.

Check the `id` matches what AFLP expects. AFLP's manifest recommends a module
with id `aflp-soundpack`. This template uses `aflp-soundpack-lite`. Use whatever
id the lite pack actually ships under so AFLP's voice engine can find it.

## How a release works

1. Commit and push these files to the repo.
2. Bump nothing in module.json by hand. The version comes from the tag.
3. On GitHub: Releases -> Draft a new release.
4. Set the tag to a clean semver like `v1.0.0` (or `1.0.0`). Tag format must be
   `v<major>.<minor>.<patch>` or `<major>.<minor>.<patch>`.
5. Publish the release.
6. The workflow runs, builds `module.zip`, fills the tokens, and attaches both
   `module.json` and `module.zip` to the release.

The Foundry install/manifest URL to share is the stable "latest" link:

    https://github.com/<owner>/<repo>/releases/latest/download/module.json

## Notes on the archive

The workflow zips the entire checkout (excluding git/CI files), so it does not
matter what your audio folders are called - everything ships. If you want a
leaner archive with an explicit file list instead, there is a commented-out
"explicit list" block in main.yml you can switch to.

## First-run checklist

- module.json is valid JSON (https://jsonlint.com/).
- The four token fields are present and spelled exactly.
- The release tag is valid semver.
- The release is Published, not just saved as draft (drafts do not trigger it).
