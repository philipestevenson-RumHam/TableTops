# RumHam Quiz Vault

Static site: lists your quiz sheets, lets people preview each PDF in-browser
(watermarked, no download/right-click), then donate via PayPal using a
reference code so you know which sheet to email them.

## Deploy to GitHub Pages
1. Create a repo, push this folder's contents to it.
2. Repo → Settings → Pages → Deploy from branch → `main` / `root`.
3. Your site is live at `https://<username>.github.io/<repo>/`.

## Add your quizzes
1. Drop each PDF into `quizzes/`.
2. Open `quizzes.json` and add an entry:
   ```json
   { "id": "q3", "title": "Sports Special", "code": "RH-SPT1", "file": "quizzes/sports-special.pdf" }
   ```
   `code` is just your own reference — pick anything memorable, it's not
   checked against anything. When someone donates and quotes the code, you
   know which PDF to email them.
3. `PAYPAL_BUSINESS` / `PAYPAL_CURRENCY` are set in `index.html` if you
   ever need to change the receiving PayPal address.

Because the page now fetches `quizzes.json`, opening `index.html` directly
as a file (`file://...`) won't load the list or the PDFs — browsers block
that fetch for local files. Preview it locally with:
```
python3 -m http.server 8000
```
then visit `http://localhost:8000`. On GitHub Pages it just works, no
server needed.

## Important — read this
This is a static site with no server, so there's no way to make a PDF truly
undownloadable. What this build does:
- Renders pages as canvas images (not a native PDF embed), so there's no
  built-in "Save As" or toolbar.
- Blocks right-click and drag on the preview.
- Stamps a watermark with the quiz code across the preview.

A determined person could still screenshot the preview or find the raw PDF
URL in the browser's network tab. This is a *deterrent*, not DRM — it's
fine for previews you don't mind being seen, but don't rely on it to
protect anything sensitive. The full, clean PDF only ever goes out by
email after payment, which is the actual protection.

If you want real access control later (expiring links, per-user gating),
that needs a backend — e.g. Supabase storage with signed URLs — which
GitHub Pages alone can't do.
