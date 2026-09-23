# Foundation for Campaign Innovation — website

A single-page static site. Everything lives in `index.html`: markup, styles, and the
handful of lines of JavaScript for the mobile menu and scroll highlighting. The only
external request is the Google Fonts stylesheet (Michroma, IBM Plex Mono, IBM Plex
Sans Condensed).

The design mirrors the Center for Campaign Innovation: the same color tokens
(`--cci-midnight #0c283a`, `--cci-blue #006db2`, `--cci-red #ff4834`,
`--cci-skye #c8e8ff`), the same typefaces, the navy diagonal-pattern page frame, the
white sidebar shell, the notched-corner cards, and the blue footer.

## Deploying to Cloudflare Pages

1. Push this folder to a GitHub repository.
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages → Connect to Git**,
   then pick the repository.
3. Build settings:
   - Framework preset: **None**
   - Build command: *(leave empty)*
   - Build output directory: `/`
4. Deploy. Every push to the default branch republishes the site.
5. Add the custom domain under **Custom domains** once the first deploy succeeds, then
   update the `og:url` and `<link rel="canonical">` values in `index.html` to match.

## Contact form

The Contact section posts to Formspree at `https://formspree.io/f/myezjnad`.

It uses the plain-HTML endpoint with a progressive-enhancement layer rather than the
`@formspree/ajax` library — there is no bundler here, and the hand-rolled version avoids
a third-party script tag on an otherwise self-contained page. The `<form>` carries a real
`action` and `method="POST"`, so it still works with JavaScript disabled (Formspree shows
its own confirmation page). When JavaScript is available, the submit handler posts with
`Accept: application/json`, keeps the visitor on the page, and writes the result into the
`#contact-status` live region.

Also included: a `_subject` hidden field to label the email, and a `_gotcha` honeypot that
Formspree uses to discard bot submissions.

Before the form delivers mail, confirm the recipient address on the Formspree form and
add the production domain to its allowed domains.

## Editing

- Copy blocks are plain HTML in the `<main>` element, one `<section>` per heading.
- Colors and type are CSS custom properties in the `:root` block at the top of the
  `<style>` element.
- `_headers` sets caching and basic security headers; Cloudflare Pages reads it at the
  root of the output directory.
