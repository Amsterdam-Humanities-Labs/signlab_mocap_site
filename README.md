# signlab_mocap_site
Static landing page at mocap.signcollect.nl that links to the motion-capture tools.

## What it does
- One `index.html` with four links: File Manager (`/animMIDI/public/`), 3D Studio (`/mocapStudio/3dOpname_test.html`), Avatar Player (`avatar.signcollect.nl/blendAnims/`), Vicon Dashboard (`/viconDashboard/`).
- Redirects to `signcollect.nl/login.html` if the `sessionObject` cookie has no `userId` (convenience gate, not security).
- POSTs `action=activity` to `signcollect.nl/users_api.php` on load.

## Where it runs
core (production): `/web/mocap_site`, vhost https://mocap.signcollect.nl

## Status
production

## How to run / deploy
Deployed by the stack: https://github.com/Amsterdam-Humanities-Labs/signlab_signcollect-stack
Locally: `python3 -m http.server 8000` (comment out the inline script, or it redirects to the production login).

## Configuration
None; every destination is a hardcoded URL in `index.html`.

## Dependencies
signlab_signCollect-v2 (`login.html`, `users_api.php`), the four linked apps, Bootstrap 5.3.2 and Font Awesome 6.5.1 from CDNs.
