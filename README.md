# signlab_mocap_site
A static start page at mocap.signcollect.nl with links to the motion-capture tools.

## What it does
- One `index.html` with four links, all on signcollect.nl unless stated:
  - File Manager: `/animMIDI/public/index.php`
  - 3D Studio: `/mocapStudio/3dOpname_test.html`
  - Avatar Player: `https://avatar.signcollect.nl/blendAnims/`
  - Vicon Dashboard: `/viconDashboard/`
- If the `sessionObject` cookie has no `userId`, it sends you to `signcollect.nl/login.html`. This is a convenience, not security.
- On load it posts `action=activity` to `signcollect.nl/users_api.php`.

## Where it runs
Core server: `/web/mocap_site`, https://mocap.signcollect.nl
Demo hosts: `<docroot>/mocap_site`, served at `/mocap_site/`. The deploy rewrites the links to same-origin paths; the Avatar Player link becomes a dead `/avatar-not-deployed` link.

## Status
Production.

## How to run / deploy
[signlab_signcollect-stack](https://github.com/Amsterdam-Humanities-Labs/signlab_signcollect-stack) deploys it. To preview locally, comment out the inline script first, or the page sends you to the production login:
```bash
python3 -m http.server 8000
```

## Configuration
None. Every link is a fixed URL in `index.html`.

## Dependencies
- [signlab_signCollect-v2](https://github.com/Amsterdam-Humanities-Labs/signlab_signCollect-v2): `login.html`, `users_api.php`.
- The four linked apps.
- Bootstrap 5.3.2 and Font Awesome 6.5.1 from CDNs.
