# Onboarding and Enablement Hub

**Status:** Draft 0.11, 17 September 2026
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

**Clinician path:** User Guide, Earn CME and Redeem MOC Credit, App Guide, Content Sources, Contact.

**Administrator path:** User Guide, Implementation Toolkit, SAML SSO Guide, EHR Integration, Link Resolver Setup, Additional Resources and Contacts.

**Nursing path:** Get Started Guide, Prompt Tips, Tips Guide, Frequently Asked Questions, all drawn from ClinicalKey for Nursing AI material.

**Resident path:** Getting the most from ClinicalKey AI, User Guide, App Guide, then a four question knowledge check with a 75 percent pass mark. Answers are marked correct or incorrect with an explanation, and the check can be retaken. The user and app guide sections are written for the resident rather than reused from the clinician path.

Every step now carries live content. No placeholders remain.

Finish path is withheld until every step in a path is marked done, and on the resident path until the knowledge check is passed. A note explains why the button is unavailable.

### Why ClinicalKey AI, and why this matters

The landing screen carries a Why ClinicalKey AI section below the path cards, covering trusted content, verifiable answers, and what makes the product fit for clinical use. It sits below the cards rather than above because the action on that screen is choosing a path; the value case supports the choice rather than blocking it.

Step one of the clinician, nurse, and resident paths opens with a professional obligation note. The register is duty of care rather than product benefit: if you rely on a tool in practice you need to know what it does and what it cannot do. The administrator path does not carry this, since an administrator is deploying the product rather than making clinical decisions with it.

### Example queries

The steps that teach question writing carry an expandable set of worked examples, shown above the embedded guide. Each pairs the situation with the query built from it, following the show do not tell principle raised in customer review.

Nurse examples are quoted verbatim from the ClinicalKey for Nursing AI prompt tips guide. Clinician and resident examples are newly drafted and are marked in the source as awaiting clinical review. They should not go in front of a customer before sign off.

Administrators see no examples, since `EXQ` has no administrator entry and the block returns nothing when the role is not covered.

### Time estimates

Every step declares a `mins` value covering the walkthrough on the page plus the guide it embeds, computed at roughly 200 words per minute for on-screen copy and one minute per page for the guides. The step rail shows a per-step figure and a path total.

Current totals are administrator 57 minutes, clinician 48, resident 28, nurse 18. Two figures are estimated rather than measured, since the Get Started and CME guides are served from Frontify with no local copy to count.

These estimates exist because faculty assigning the hub as coursework need a time cost before they can assign it, and because accreditation limits how much work can be set outside teaching hours.

### Sharing

Two share actions, deliberately separate so reporting can tell them apart.

**Share this hub** sits in the top bar and copies `SHARE_BASE`, the address of the experience as a whole. In a production build it occupies the space the learning record badge uses during testing.

**Share this guide with a colleague** sits in the guide row beside View full size and Download this guide, and copies the Frontify address of that asset. This is the moment-of-need case: someone who did not know an app existed, or that CME accrues automatically, gets the guide rather than the tour.

Both send a shared statement, one against the hub and one against the guide, so it is possible to read whether people pass on the experience or the asset.

Per-step links are not offered. Every screen in the hub sits at the same address, because the experience is a page inside a WordPress page and script here cannot read or write the parent URL. Sharing the hub address covers the great majority of cases, and sharing the guide covers the rest.

The receiving half of deep linking is present and dormant. `readDeep` reads a `#role/step-slug` fragment on this document and, after the gate, holds the requested step while merging saved progress. It works when the hub is opened directly and will start working through the embed on the day the parent page forwards its fragment to the iframe `src`. Nothing currently generates such a link.

`SHARE_BASE` is a constant. If the hub moves, it must be updated by hand. The iframe tag also needs `allow="clipboard-write"` or copying falls back to a prompt dialog.

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

**Clinician example queries.** Newly drafted, not sourced from an approved guide. Marked in the source. Needs clinical review before external use.

**Deep linking.** The receiving code is in place but dormant. It needs the WordPress page to forward its fragment to the iframe before a per-step link can exist. Owned by the WordPress contact, not by this file.

**Schedule a call.** The label is under review. It routes to the Resource Center customer support page rather than a booking tool, and the wording may be promising more than it delivers.

**Nurse content sources.** The nurse path has no content sources step. That corpus belongs to ClinicalKey for Nursing AI and needs its own list rather than a trim of the ClinicalKey AI one.

**Mobile.** Desktop first by decision, not by oversight. Mobile rendering through the WordPress embed is a known problem and is scheduled as its own version.

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

**Draft 0.11, 17 September 2026**
First commit in a new repository. Identity, credentials, and the landing screen.

- Learner email addresses are hashed with SHA-256 in the browser. Only the hash is sent, as the name of an xAPI account object rather than an mbox, and the same hash keys the State API. The address itself is never stored. See the identity block in the source for what this does and does not buy
- Hashing fails closed. If a browser cannot do Web Crypto the gate stops and says so rather than falling back to sending the address in the clear. This requires HTTPS, so opening the file over `file://` will trip that message
- LRS credentials removed from the source and replaced with three labelled placeholders. They are filled in locally from a dedicated Activity Provider set to Read/Write with Allowed Endpoints restricted to the hub app
- `TESTING` set to false, so the testing banner, the learning record badge and the badge explainer are hidden. It remains a one word edit to turn back on
- Brand layering device added to the landing screen. Portrait rectangles at 3 by 4, one image per rectangle, offset in sequence, with a single Vital Orange rectangle. Static, decorative, and hidden from assistive technology
- Hero and both role cards moved into a left column with the stack beside them, so the screen reads at one level. Card padding and type reduced to match
- Gate callout now explains that the address is converted to a one-way code in the browser. The guidance to use an institutional address where one exists is unchanged
- The nursing coming soon screen, its stylesheet and the nursing logo removed. Nothing had called it since the nurse path came out in 0.10

### Identity is not reversible

Hashing is one way. Given a candidate address you can hash it and match a record, which covers deletion and access requests. You cannot work back from stored hashes to a list of people, and you cannot email an individual learner. Institution level reporting is unaffected and runs on the organization field.

`ID_HOMEPAGE` and `ID_SALT` both form part of the identifier. Changing either orphans every existing progress record.

### The credential in this file is public

The file is served to every browser, so anything in it can be read in developer tools, and the Basic auth header appears in the network tab on every request regardless. The Activity Provider scoping is what limits the damage, not secrecy. There is no way to remove the secret from a browser-only build; that needs a server-side proxy.

**Draft 0.10, 17 September 2026**
Streamlined build. Four paths become two, the knowledge check comes out, and the wrap up becomes a destination rather than a button.

- Nurse and resident paths removed, along with the ClinicalKey for Nursing assets, the resident guide copy, the knowledge check, and the quiz machinery. Nursing becomes its own hub, structured ClinicalKey for Nursing then ClinicalKey for Nursing AI, with user and administrator beneath each. Resident was a proof of concept
- Two paths remain, administrator and clinician, at eight content steps each
- The intro video sits on the clinician path only. An administrator is deploying the product rather than using it, and in most cases has already seen a tour. It was considered for the gate and for the path selection screen and rejected in both places
- Wrap up added as a terminal entry in the step rail, unavailable until every content step has been viewed. Hovering or tapping it while locked names what is outstanding
- News and notes lives at the wrap up rather than up front, so a learner arrives at it instead of detouring to it, and does not leave the experience to read a newsletter. Sourced from the June and August 2026 customer newsletters and the Q2 2026 quarterly flyer. Links deliberately omitted in this draft
- Step numbers replaced with dots. Numbers implied a required sequence in an experience that is deliberately open. A viewed step fills its dot, carries a small check reading Viewed, and shifts to Paper
- Mark done and continue becomes Viewed, next step, and Viewed, wrap up on the last content step. Viewing is what the record can attest to; understanding is not
- Advancing out of order routes to the first step still unseen rather than to a wrap up that is not open
- Gate rebuilt to two blocks on the left, the hub and Why ClinicalKey AI. Why moved here from the landing screen. The eyebrow and the learning record explainer come out of the white box
- Landing screen is now the hero and two cards
- Admin path renamed Administrator path in the rail

Deliberately unchanged: the gate fields, the progress model, `TESTING`, and `ACT_BASE`.

### Known, and not resolved

News and notes goes stale. It has no named owner and no refresh cadence. The newsletters behind it are June and August, the flyer is Q2, and this is customer facing. This is the same staleness risk already logged against the docs folder, with a shorter fuse.

**Draft 0.9.1, 31 August 2026**
Review with Laura, 31 August, ahead of showing John.

- Why ClinicalKey AI moved up and condensed so the landing screen reads without a scroll. The separator rule above it is gone, the heading is larger, and Why is italicized to rhyme with start in the hero
- Value line rewritten. Acceleration layer, not an authority was cut: it is not language clinicians use, and it undercuts the point, since an authoritative answer is exactly what someone is looking for. The line now reads as one thought ending at traceable to a source
- Share moved and split in two. Share this hub sits in the top bar, in the space the learning record badge occupies during testing. Share this guide with a colleague sits in the guide row. Separate statements, so passing on the experience and passing on the asset can be read apart
- Copy link to this step removed. Every screen shares one address inside the WordPress page, so a per-step link could not have worked. The receiving code stays, dormant
- Bold labels removed from the nursing prompt tips and FAQ bullets, and the sentences rewritten as complete thoughts. The bold was catching the eye and stopping the read before the end of the line
- Content sources now uses one text colour throughout. The title lists were a shade darker than the lines above and below them
- User path renamed Clinician path in the step rail and the role pill. The entry card already said clinician

**Draft 0.9, 31 August 2026**
Customer review response. Built from the Quinnipiac session on 25 August, the review with Laura on 26 August, and the stand-up with John the same day.

- Landing lede now reads "the path that fits your role, or the role you are training for," so a student who is not yet a clinician or resident has an entry. Suggested by the customer in review, whose institution is predominantly students
- Why ClinicalKey AI section added below the path cards, drawn from the value story in the CSM Knowledge Center. Trusted content, verifiable answers, built for clinical use, led by the acceleration layer positioning
- Professional obligation note added to step one of the clinician, nurse, and resident paths. Administrator excluded
- Example queries added to the steps that teach question writing, as an expandable above the guide, with a link through to the product. Nurse examples quoted from the prompt tips guide; clinician and resident examples drafted and awaiting clinical review
- Key actions heading raised from 14.5px to 19px, sitting between body copy and the page heading. Requested in review as the one specific cosmetic change
- Content sources rebuilt from four categories to five, each now carrying a representative title list rather than a description alone. Point of care content, journals, reference books, clinical practice guidelines, government and regulatory. Editions omitted so the page does not date on the next release, and the master content list remains authoritative
- Opening copy on content sources now states that the library draws on Elsevier and non-Elsevier sources, which is the strongest thing that section can say
- Time estimate added to every step and totalled per path, covering the page plus the guide it embeds
- Gate copy corrected. It claimed roughly 20 to 30 minutes for a path; the real range is 18 to 57 minutes
- Copy link to this step added on every step, with a shared statement to the learning record store and inbound routing that holds the requested step after the gate
- Resident path expanded from two steps to four, adding User Guide and App Guide written for the resident. Requested so the knowledge check reads as a differentiator against a path with content in it rather than a single page
- Knowledge check unchanged at four questions on the original content, per instruction

Deliberately unchanged: the gate, the progress model, the four path structure, `TESTING`, and `ACT_BASE`.

**Draft 0.8, 19 August 2026**
Reviewed live with John, 19 August.

- Gate and landing heading changed to "Welcome to your Elsevier digital experience," per his suggestion on the call
- Content Sources is now its own step on the clinician path, rendered as four collapsible sections directly on page rather than a link out. Two further reading links to the Resource Center sit below the accordion for a full title level list
- Contact added as a final step on the clinician path, mirroring the Support Center and schedule a call cards from the administrator resources page
- Content sources on the administrator path is unchanged, since John was comfortable with it as is there
- Added a "Driving adoption at your organization" section to Additional Resources and Contacts on the administrator path, in its own box rather than under further reading. Link and description confirmed by John, 19 August; description written from the live Resource Center page rather than a placeholder
- Clinician path is now eight steps

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
