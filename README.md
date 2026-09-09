# mocap_site

The Motion Capture portal: the single page users land on at
`mocap.signcollect.nl`, which sends them on to the right motion-capture tool.

> **Naming note:** every other repo in this org is prefixed `signlab_`. This one
> was transferred in before that convention existed, and renaming it needs
> org-owner rights. Read it as `signlab_mocap_site`.

## What it does

One static HTML file, and nothing else. It renders a card with four links —

| Link | Goes to | Purpose |
|---|---|---|
| Motion Capture File Manager | `signcollect.nl/animMIDI/public/index.php` | post-processing management |
| 3D Studio Capture Site | `signcollect.nl/mocapStudio/3dOpname_test.html` | live capture sessions |
| signCollect Avatar Player | `avatar.signcollect.nl/blendAnims/` | avatar animation playback |
| Vicon Dashboard Sync | `signcollect.nl/viconDashboard/` | sync dashboard |

— and does two things in a small inline script on load:

1. **Gate on the session.** It reads the `sessionObject` cookie; if there is no
   parsed `userId` in it, it redirects to `signcollect.nl/login.html` with the
   current URL as `redirect`. This is a convenience gate, not a security
   boundary — the target sites do their own authorisation.
2. **Record the visit.** It POSTs `action=activity` with the user id and the
   current page to `signcollect.nl/users_api.php`, feeding the estate's
   user-activity tracking.

There is no build step, no framework, no backend of its own. Bootstrap and
Font Awesome come from public CDNs.

## Where it runs

**The signcollect core server (production VPS).** It is served as a static site
at `mocap.signcollect.nl` and deployed to `/web/mocap_site` on that host. The
APIs it calls (`users_api.php`, `login.html`) live on the same estate under
`signcollect.nl`.

## Status

**Production.** It is the front door to the motion-capture tooling; a broken
deploy here means users cannot find any of the four sites above.

## How to run it

Locally, open `index.html` in a browser, or serve the directory over HTTP:

```bash
python3 -m http.server 8000    # then http://localhost:8000/
```

Note that both runtime behaviours reach out to `signcollect.nl` regardless of
where the page is served from: without a valid `sessionObject` cookie for that
domain, a local copy will immediately redirect you to the production login page.
Comment out the inline script at the bottom of `index.html` while working on the
layout.

To deploy, put the file at `/web/mocap_site/index.html` on the core server. No
build, no restart, no cache to clear.

## Configuration

None. There is no config file, no environment variable and no credential — every
destination is a hardcoded absolute URL in `index.html`. Adding or moving a tool
means editing that file.

The `.gitignore` guards against the estate's usual secrets (`mysql_config.php`,
`.env`, `*.pem`) being dropped into this directory by mistake; none of them are
used by the page.

## Dependencies

- **The core server's web root** (`/web`) and the `mocap.signcollect.nl` vhost.
- **`signcollect.nl/users_api.php`** and **`login.html`** — the session cookie
  and activity-tracking endpoints. If either moves, this page breaks silently
  (the redirect loop) or loses tracking.
- **The four linked applications**, each its own repo/deployment: `animMIDI`,
  `signlab_mocapStudio`, the avatar player at `avatar.signcollect.nl`, and
  `signlab_viconDashboard`.
- **CDNs**: Bootstrap 5.3.2 (jsDelivr, SRI-pinned) and Font Awesome 6.5.1
  (cdnjs). Both are required for the page to look right; it degrades to unstyled
  links without them.
