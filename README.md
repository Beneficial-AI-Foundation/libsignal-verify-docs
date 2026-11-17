# libsignal-verify-docs

This repository builds the blueprint book of
[libsignal-verify](https://github.com/Beneficial-AI-Foundation/libsignal-verify) and deploys it to
GitHub Pages. The book states what the Lean development proves about PQXDH, the key agreement
protocol of Signal, and what it does not prove.

The sources of the book are not here. They are in the folder `vb-docs/` of libsignal-verify. This
repository holds only the workflow `.github/workflows/pages.yml` and the file
`LIBSIGNAL_VERIFY_REF`.

## Which commit is deployed

`LIBSIGNAL_VERIFY_REF` holds the commit of libsignal-verify that the workflow builds. Nothing runs
by itself: a push to libsignal-verify does not change the site. The text of the book is written
against one commit, and the "Source file" links of the book name that commit.

## Update the deployed book

1. Choose a commit of libsignal-verify that contains `vb-docs/scripts/export-public.py`.
2. Write its full 40-digit hash into `LIBSIGNAL_VERIFY_REF` and push to `main`.
3. Open Actions, choose "Build and deploy the libsignal-verify blueprint book" and select
   "Run workflow".

To try a commit without changing the file, enter it in the `ref` field of "Run workflow". The
site then shows that commit until the next run.

The workflow stops with a message if the commit has no `vb-docs/scripts/export-public.py`.

## One-time set-up

In Settings, Pages, set the source to "GitHub Actions".

## What the workflow does

1. Checks out libsignal-verify at the chosen commit and writes that commit into
   `vb-docs/source-revision`, from which the book takes the commit for its source links.
2. Installs the Lean toolchain that libsignal-verify pins, restores a cache of the compiled
   packages, and fetches the prebuilt Mathlib.
3. Runs `lake build LibsignalVerifyBlueprint` and `lake exe vbp build` in `vb-docs/`.
4. Runs `vb-docs/scripts/export-public.py`, which removes every local path from the site and fails
   if one remains.
5. Uploads the site and deploys it.

To build the book on your own machine, follow `vb-docs/README.md` in libsignal-verify.
