# Onboarding and Enablement Hub

**Status:** Draft 0.7, 19 August 2026
**Stage:** Internal review. Not for customer distribution.

A self-service onboarding and enablement experience for ClinicalKey AI customers in the self-support tier, meaning accounts without a dedicated Customer Success Manager. A learner picks a path, works through a sequence of steps, and can jump straight to a topic using the browse by concern panel.

Built as a single self-contained HTML file so it can be hosted statically and embedded anywhere.

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire experience. Markup, styles, and logic in one file. |
| `/fonts` | Six licensed brand web fonts, referenced by `@font-face` with relative paths. Cleared for public hosting. |
| `/assets` | Elsevier primary logo, favicon, and the ClinicalKey for Nursing AI product logo. |
| `/docs` | Product guides served to the inline viewer, plus the supporting resources linked from See also. See the note on hosting below. |
| `README.md` | This file. |

`index.html` must keep that exact name. Static hosts look for it by default, and renaming it produces a 404 with no other symptom.

---

## Hosting

Served as a static page from this repository and embedded into WordPress through an iframe. A script embed was tested first and renders blank, because WordPress strips styles and scripts. The iframe is the fix, not a preference.

To publish, commit to the default branch and confirm the build completed under the Actions tab. The live address is under Settings, then Pages.

---

## What is in the build

Four paths. The clinician and administrator paths share an opening block, then diverge. The nursing and resident paths stand alone.

**Shared opening:** an introductory video, the Get Started Guide, and Frequently Asked Questions.

**Clinician path:** User Guide, Earn CME and Redeem MOC Credit, App Guide.

**Administrator path:** User Guide, Implementation Toolkit, SAML SSO Guide, EHR Integration, Link Resolver Setup, Additional Resources and Contacts.

**Nursing path:** Get Started Guide, Prompt Tips, Tips Guide, Frequently Asked Questions, all drawn from ClinicalKey for Nursing AI material.

**Resident path:** one content page, then a four question knowledge check with a 75 percent pass mark. Answers are marked correct or incorrect with an explanation, and the check can be retaken.

Every step now carries live content. No placeholders remain.

Finish path is withheld until every step in a path is marked done, and on the resident path until the knowledge check is passed. A note explains why the button is unavailable.

### How guides are served

Each guide has two routes. The inline viewer reads a copy committed under `/docs`, and the download action opens the Frontify share page.

This split exists because a Frontify share page is a viewing wrapper rather than a direct asset URL, so it cannot be rendered by the inline viewer. Two guides, Get Started and the CME and MOC guide, use direct Frontify asset URLs instead and need no local copy.

The committed copies are a working arrangement, not the end state. Replacing them with direct Frontify asset URLs would remove the duplication and the risk of the hub serving a stale revision after a guide is updated.

---

## Known open items

These are decisions held open on purpose, not defects.

**Entry screen.** The hub opens on a gate collecting email and organization, both required. Email identifies the learner so progress can be saved and resumed. Organization is free text with no validation, so the data is self reported. During the testing phase reviewers are asked to enter a placeholder email address.

**Learning record store.** Statements are sent to a sandbox endpoint. Not production, and not suitable for real data.

**Activity base.** `ACT_BASE` still holds a placeholder value, so activity identifiers do not yet point at a settled address. Changing it splits reporting between old and new identifiers, so it should be set once, deliberately, when hosting is final.

**Progress.** Saved to the learning record store through the xAPI State API, keyed to the email entered at the gate. Returning with the same address offers a resume prompt rather than jumping straight back in. If the store is unreachable the hub still works, it just will not remember anything.

**Section order.** Whether Frequently Asked Questions belongs in the journey or as a link out to the Support Center is still being decided.

---

## Brand

Two typefaces. Tiempos Text for headlines, National 2 for body, UI, and small text. Georgia and Arial appear in the fallback stacks and are availability fallbacks only, never the intended typeface.

Palette is Graphite, White, Paper, Sand, and Ink as the foundation, with Vital Orange as a sparing accent and Action Blue reserved strictly for interactive elements. Ivory is for lines and dividers. All text meets WCAG AA contrast against its background.

The `/fonts` folder must travel with `index.html`. Without it the page falls back to Georgia and Arial and stops being on brand.

---

## Updating this repo

1. Open the Code tab.
2. Use the plus sign next to Code, then Upload files.
3. Drag the file in, or choose it.
4. Add a short commit message describing what changed. Worth doing for anything beyond a typo.
5. Commit.
6. Check the Actions tab to confirm the build succeeded, then reload the live page.

Uploading a file with the same name replaces it. Deleting is done from the file itself, through the ellipsis menu, and also requires a commit.

---

## Change log

**Draft 0.7, 19 August 2026**
Product-agnostic shell.

- Renamed to Onboarding and Enablement Hub, and the eyebrow on both the gate and
  the landing screen now reads Customer Success, Your Digital Experience
- The hub framing no longer names a product. Gate, introduction, hero and lede
  describe the model rather than ClinicalKey AI, so the shell can be reused
- Role card descriptions still name what each path covers, since a nurse needs to
  know the nursing path is a different product
- Content sources and the master content list added to the resources page as their
  own block, linked to the Resource Center rather than reproduced
- Added a TESTING switch at the top of the script. Set it to false and the testing
  banner, the learning-record badge and the badge explainer all disappear

**Draft 0.6, 17 August 2026**
Named, introduced, and complete across four paths.

- Renamed to ClinicalKey AI Learning Hub throughout
- Gate rebuilt as two columns: a description of what this is and what to expect beside the form, so someone arriving from a link has context
- Progress note moved from fine print into a callout, and the institution example changed to a generic one
- Nursing path built out with four ClinicalKey for Nursing AI guides, replacing the coming soon page
- Added Additional Resources and Contacts to the administrator path: the Support Center, a route to schedule a call, and three further reading pieces
- Supporting resources consolidated onto that page and removed from inline See also, so there is one place to maintain them
- Resources are administrator only
- Removed the API integration piece and both clinician trust interviews
- Highspot links replaced with Frontify throughout
- Certificate button added on a passed knowledge check
- Clinician card no longer says user
- Video introduction copy no longer describes the path instead of the video

**Draft 0.5, 17 August 2026**

- Four See also resources now link to Highspot rather than hosted copies, removing 7.4MB from the repository
- The Strzoda interview is still served locally, pending a Highspot link

Known issue: the Highspot URLs are Pitch links rather than evergreen content
links. They carry view tracking back to the pitch that created them and can be
revoked or expire. Raised with John. Replace when permanent links exist.

**Draft 0.4, 14 August 2026**
Supporting resources surfaced through See also.

- Add a See also block that appears after the embedded guide on sections where a supporting resource genuinely adds something, rather than on every section
- FAQ links to two clinician interviews on what makes AI trustworthy
- Implementation Toolkit links to the ViVE 2026 adoption case study and the Society of Hospital Medicine round table white paper
- EHR Integration links to the API integration overview and the public developer documentation
- Resident path links to the resident interview and the round table white paper
- See also links send their own xAPI statement, distinct from guide downloads, so interest in supporting material can be read separately from use of the guides

**Draft 0.3, 12 August 2026**
Live content throughout, and a brand compliance pass.

- All remaining placeholders replaced with live guides and written walkthroughs: FAQ, User Guide, Implementation Toolkit, SAML SSO Guide, EHR Integration, Link Resolver Setup
- Organization field now asks for the full name and flags likely abbreviations, since abbreviations make institutions indistinguishable in reporting
- Brand palette enforced. Status colours that sat outside the palette were replaced with brand tokens
- Correct and incorrect answers now carry a symbol as well as a colour, so the distinction does not depend on colour perception
- Small text no longer sits directly on Vital Orange, which fell short of the AA threshold. Orange is now confined to rules, borders, and large display text
- Letter spacing set to the brand standard throughout
- Every text and background pair verified against WCAG AA

**Draft 0.2, 12 August 2026**
Four paths, assessment, and progress that persists.

- Nursing and resident entry cards added, so the hub now carries four paths
- Nursing path is a single coming soon page for ClinicalKey AI for Nursing
- Resident path is one content page followed by a four question knowledge check, 75 percent to pass, with per question feedback and unlimited retakes
- Finish path is withheld until every step is marked done, with a note explaining why
- Progress moved from session memory to the xAPI State API, with a resume prompt on return
- Step completion keyed by a stable slug rather than array position, so reordering steps no longer corrupts saved progress
- Organization added to the gate as a required field, and carried on every statement as context
- App Guide added with a written walkthrough
- Document viewer now renders at three times display resolution, with a full size view that hides the rail and finder
- Credit and records renamed to Continuing education
- Switch path added to the header
- Elsevier logo, favicon, and product logo assets added

**Draft 0.1, 11 August 2026**
First revision following stakeholder review.

- Entry card bullets rewritten on both paths, with CME and MOC given their own line
- Epic Integration with SSO renamed to EHR Integration throughout, so the section is not tied to a single vendor
- Setting Up Conversation Sharing removed, since sharing is enabled by default and opting out is handled by support request
- Placeholders now name the awaited asset and its owner
- Version stamp added to every screen
- Entry screen left unchanged pending a decision on the email gate

**Prototype, prior**
Initial build. Two paths, step rail, browse by concern panel, paginated document viewer, and xAPI statement tracking.
