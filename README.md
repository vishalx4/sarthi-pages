# Sarthi public pages

Three static pages that exist so the Google Auth Platform consent screen can
be moved from **Testing** to **In production**. Publishing requires a live
home page, privacy policy and Terms of Service URL; while the project stays in
Testing, every refresh token Google issues expires after 7 days, which breaks
Google Meet link creation on bookings once a week.

Plain HTML and one stylesheet — no build step, no web fonts, no external
requests, so the pages are always reachable.

## Publish on GitHub Pages

1. Create a **public** repo, e.g. `sarthi-pages`. It holds only these files —
   the application repos stay private.
2. Push:
   ```
   git init && git add -A && git commit -m "Public pages for OAuth consent screen"
   git branch -M main
   git remote add origin git@github.com:<you>/sarthi-pages.git
   git push -u origin main
   ```
3. Repo → **Settings → Pages** → Source: *Deploy from a branch*, Branch:
   `main` / `/ (root)` → Save. The URL appears within a minute or two.

## Then, in Google Cloud Console

**Branding** — fill in, using your Pages URL:

| Field | Value |
|---|---|
| Application home page | `https://<you>.github.io/sarthi-pages/` |
| Application privacy policy link | `https://<you>.github.io/sarthi-pages/privacy.html` |
| Application Terms of Service link | `https://<you>.github.io/sarthi-pages/terms.html` |
| Authorised domains | `github.io` |

Leave the **app logo empty**. Uploading one forces the app into Google's
verification process, which these pages are specifically designed to avoid.

**Audience** → **Publish app** → status becomes *In production*.

Then re-mint the refresh token — the existing one was issued under Testing and
keeps its 7-day life regardless of the new status. See
`backend/scripts/mint-google-refresh-token.py`.

## Before a real launch

- Replace the GitHub Pages URLs with your own domain once you have one, and
  update Authorised domains to match.
- Replace `meetmentor5151@gmail.com` with a branded support address. It
  appears on all three pages.
- Have a lawyer read the policy and terms. They describe this system
  accurately — what is collected, that scorecards are parsed and discarded,
  that Google and MSG91 receive specific fields — but accuracy is not the same
  thing as legal sufficiency for your jurisdiction and payment model.

## Keep these accurate

The privacy policy lists real fields from `MentorApplicants`,
`GuideBookings`, the credential tables and the session record, and names the
two processors that receive personal data (Google Calendar/Meet for invites,
MSG91 for OTP delivery). **If you add a data field, an upload that gets
stored, or a third-party service, update `privacy.html` in the same change.**
