# Nocturn tag site (GitHub Pages)

The Nocturn app's NFC tags hold a universal link like
`https://thisisthechris.github.io/nocturn/t/04A1B2C3D4E580`, which contains the tag's own UID.
iOS only opens the app from a background tag read if this domain serves the
`apple-app-site-association` (AASA) file in this folder.

## Publish

1. On GitHub, create a **public** repository named exactly `thisisthechris.github.io`.
   It has to be the user site, because the AASA file must be at the domain root.
2. Copy the *contents* of this folder (including the hidden `.well-known/` folder and `.nojekyll`) to the root of that repository:

   ```sh
   git clone https://github.com/thisisthechris/thisisthechris.github.io.git
   cp -R tag-site/. thisisthechris.github.io/
   cd thisisthechris.github.io && git add -A && git commit -m "Add Nocturn tag links" && git push
   ```

3. In the repository go to **Settings → Pages** and deploy from the `main` branch, `/ (root)`.
4. Check that the file is served over HTTPS with no redirects:

   ```sh
   curl -sI https://thisisthechris.github.io/.well-known/apple-app-site-association
   curl -s  https://app-site-association.cdn-apple.com/a/v1/thisisthechris.github.io
   ```

   The second command shows what Apple's CDN has cached. It can take a while to update after the first publish.
   While you wait, you can test on the iPhone with **Settings → Developer → Associated Domains Development**.
   This only applies with a `?mode=developer` entitlement, so normally you just wait for the CDN.

`.nojekyll` stops GitHub Pages ignoring the dot-folder. `404.html` is shown if a tag is
opened on a phone without the app.

If the app's team ID or bundle ID changes, update `appIDs` in
`.well-known/apple-app-site-association`. If the domain changes, also update `TagLink.host`
and `Nocturn2.entitlements`.
