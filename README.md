# Ashby — the Butler Cat

A one-eyed grey cat who behaves like a British butler, voiced by a small language model
([Qwen2.5-0.5B-Instruct](https://huggingface.co/onnx-community/Qwen2.5-0.5B-Instruct), Apache-2.0)
running entirely in the visitor's browser with [Transformers.js](https://huggingface.co/docs/transformers.js) 4.3.2.
WebGPU is used where available, with a WASM (CPU) fallback.

The whole app is one file, `index.html`.

## Deploy to GitHub Pages

1. Merge this branch into `main`.
2. In the repository, go to **Settings → Pages** and under **Build and deployment → Source** choose **GitHub Actions**.
3. The workflow in `.github/workflows/pages.yml` runs on every push to `main` (or run it by hand from the
   **Actions** tab). It publishes only `index.html` and `.nojekyll`, so other files in the repository are not put on the site.
4. The site is live at `https://<username>.github.io/<repo-name>/`.

Alternative without Actions: choose **Deploy from a branch**, branch `main`, folder `/ (root)`.
This publishes every file in the repository root, so anything there is public on the site.

## Notes

- **Download size:** about 483 MB on most WebGPU browsers (786 MB on WebGPU without 16-bit float
  support, 512 MB on the WASM fallback); the page looks up the exact size from Hugging Face when it loads.
  Weights come from Hugging Face's CDN, not GitHub, so they don't count against GitHub Pages'
  size or bandwidth limits.
- **Downloaded once:** weights are kept in the browser's Cache Storage, and the page asks the browser to
  keep that storage persistent. Clearing site data removes them.
- **Context length:** only the last 3 exchanges are sent to the model (`MAX_EXCHANGES`), each message
  clipped to 600 characters (`MAX_CHARS_PER_MSG`). Both are at the top of the main script in `index.html`,
  with the character prompt (`SYSTEM_PROMPT`) and generation settings.
- **WASM speed:** GitHub Pages can't send the cross-origin isolation headers that multi-threaded WASM
  needs, so the CPU fallback runs single-threaded. WebGPU is unaffected.
- **Local testing:** serve the folder (for example `python3 -m http.server`) rather than opening the file directly.
