# Focused neon/glass repair

Status: ready for review, not deployed. Scope: the two static pages loading assets/css/future-buttons.css; not the separate Next.js app.

## Changes

Preserved existing CSS and appended bounded overrides. No JavaScript, content, authentication, dependencies, or production configuration changed.

- Console header now wraps its long runtime identifier and retains status lights. At 320px, browser comparison with the repair rules removed measured header scrollWidth 293px; with repairs, 286px.
- Cards use a container-bounded minimum track width. Grid/flex children can shrink; CTA labels and panel text can wrap. This is defensive sizing, not a claim that the original 270px cards overflowed at 320px.
- Shared native focus-visible rings work before progressive button enhancement.
- Reduced-motion preference now suppresses hover/pressed transforms and decorative sweeps, and disables page smooth scrolling.
- Opaque glass fallback for unsupported backdrop filtering; composite-mask fallback hides the decorative edge instead of painting an unmasked gradient.
- Existing palette retained. Added --cg-glass-solid (#09182b), --cg-focus-ring (#9bf6ff), --cg-edge (rgba(151,246,255,.48)). No new infinite animations or dependencies.

## Executed verification

Checkout baseline: 1855f58b8494906978d0be8f237247f7521d1504.
Implementation commit: fe9609c5659561776fb59e55853ecacdda8dfa0d.

- git diff --check HEAD^ HEAD: pass.
- python3 security/validate_static_site.py: Static-site validation passed.
- python -m pytest tests/test_static_site.py -q: could not run; python command missing.
- python3 -m pytest tests/test_static_site.py -q: could not run; pytest module missing.
- python3 -m http.server 8765: served static pages successfully.
- agent-browser --session neon-repair open http://localhost:8765/CG-os.html and /index.html; set viewport WIDTH 900 for 320, 375, 768, 1024, 1440, 1920: document scrollWidth equaled viewport at all 12 combinations. No internal overflow found in .cards, .consoleHead, .ctaRow using scrollWidth > clientWidth.
- agent-browser --session neon-repair set media dark reduced-motion; focus and hover .ctaRow .primary: prefers-reduced-motion true, html scroll-behavior auto, transform none, translate none, transition duration 0s, focus outline solid 2px.
- agent-browser --session neon-repair errors on flagship page: no reported page errors.

These are headless Chromium layout/computed-style checks, not screenshot-based visual approval or physical-device testing. Body overflow hiding still exists in original styles; document width alone is not proof every descendant is unclipped.

## Remaining verification

Not verified: physical mobile devices, Safari/Firefox, unsupported-backdrop/mask engines, forced-colors rendering, full keyboard traversal, contrast audit, production deployment, performance profiling, separate Next.js app build. No full redesign or portfolio-wide completion claim.

## Rollback and next action

Review the appended section in future-buttons.css and run visual/browser coverage before approving. Roll back by reverting the repair commits. No merge or deployment was performed.
