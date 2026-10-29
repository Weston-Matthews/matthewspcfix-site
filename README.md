# matthewspcfix.com — static site for GitHub Pages

Plain HTML/CSS rebuild of the Matthews Software & Computer Repair site, moved off
Square Online / Weebly. No build step, no framework: what's in this folder is what gets served.

```
index.html  services.html  about.html  contact.html   # same page names/URLs as the old site
css/style.css                                         # all styling
images/                                               # images downloaded from the old site
CNAME                                                 # tells GitHub Pages the custom domain
.nojekyll                                             # serve files as-is (skip Jekyll)
```

Preview locally: open `index.html` in a browser, or run `python3 -m http.server` in this folder
and visit http://localhost:8000.

## Deploy on GitHub Pages

1. Create a new **public** repo on GitHub (e.g. `matthewspcfix-site`). Free GitHub Pages
   requires a public repo.
2. Upload the contents of this folder to the repo root (drag-and-drop in the GitHub web UI,
   or `git init && git add . && git commit -m "Initial site" && git push`).
   Make sure `CNAME` and `index.html` are at the top level of the repo, not inside a subfolder.
3. Repo **Settings → Pages**: Source = *Deploy from a branch*, Branch = `main`, folder `/ (root)`. Save.
4. Still in Settings → Pages, **Custom domain** should show `matthewspcfix.com` (from the CNAME file).
5. Optional but recommended: verify the domain under your GitHub account
   (**Settings → Pages → Add a domain**) so nobody else can claim it.
6. Set the DNS records below at Namecheap. Once GitHub's DNS check passes and the certificate is
   issued (can take up to ~24h), tick **Enforce HTTPS**.

## Namecheap DNS records

Namecheap → Domain List → matthewspcfix.com → **Manage → Advanced DNS → Host Records**.

Remove the old website records first: the existing `A @ 159.198.67.192` and `A www 159.198.67.192`
(Square/Weebly), plus any URL Redirect record for `@` or `www`. Then add:

| Type         | Host | Value                       | TTL       |
|--------------|------|-----------------------------|-----------|
| A Record     | @    | 185.199.108.153             | Automatic |
| A Record     | @    | 185.199.109.153             | Automatic |
| A Record     | @    | 185.199.110.153             | Automatic |
| A Record     | @    | 185.199.111.153             | Automatic |
| CNAME Record | www  | `<username>.github.io.`     | Automatic |

Replace `<username>` with your GitHub username (the CNAME points to the user site host, **not**
including the repo name).

**Don't touch the email settings.** The domain currently uses Namecheap Email Forwarding
(MX records `eforward1–5.registrar-servers.com` and the TXT record
`v=spf1 include:spf.efwd.registrar-servers.com ~all`). Leave "Mail Settings" on *Email Forwarding*
and keep those records. There's also a TXT record `53acc1c0-666c-11f0-b1a4-91ed15e90a80` that
is probably a verification token from the old host; it's safe to leave it in place.

Check propagation:

```
dig +short matthewspcfix.com A        # should list the four 185.199.x.153 addresses
dig +short www.matthewspcfix.com      # should show <username>.github.io
dig +short matthewspcfix.com MX       # should still show the eforward records
```

## Contact form

The old site had a Weebly contact form, which stops working once the site leaves Weebly.
`contact.html` uses `mailto:` links instead, and also has a commented-out
[Formspree](https://formspree.io) form with the same fields. To turn it on, create a free
Formspree form, put your form ID in place of `YOUR_FORM_ID`, and uncomment the block.

## After it's live

Once the new site works on https://matthewspcfix.com, cancel the Square Online plan
($17.06/mo). Make sure the domain stays registered at Namecheap; only the website plan should be cancelled.
