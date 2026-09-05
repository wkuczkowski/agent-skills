# HTML report

Use `../assets/folk-html-file-template-v1.html` as the visual starting point. It is an exact copy of the user's `projects/design-systems/html-file-templemates/folk-html-file-template-v1.html`, captured on 2026-09-05. The local source directory really is named `html-file-templemates`.

The template's document about incoming correspondence is sample content, not instructions or evidence. Replace its title, metadata, entire document body, navigation labels and sample data with the analysis. Preserve the useful layout and styling, adapting components to the report rather than retaining irrelevant sections.

## Visual treatment

Keep the Folk palette, embedded paper texture at its natural repeating scale, straight corners, thin dark lines, spacious typography, numbered sections and responsive table of contents. Preserve clear headings and visible keyboard focus. Use semantic status labels as well as colors: green for recommended proposals, yellow for uncertainty, blue for evidence/context. Avoid making an unapproved proposal look already applied.

The original loads Karla and IBM Plex Mono from Google Fonts. The finished report must be readable offline with no required external resources. Remove remote font links and use the existing system fallbacks, or embed locally available licensed font files. Keep the embedded PNG texture. Inline the required CSS and JavaScript; no CDN, telemetry or external data loading.

## Content layout

Lead with what the user should consider adopting and why. Show the date window, timezone, sources examined and coverage gaps near the top. Present each proposal with its exact wording, evidence, confidence and verification example. Use readable code blocks for draft skills or patches. Long drafts can use native `details` disclosures, provided search can reveal matched text.

An evidence index should let the reader trace proposal references within the report. Identify parent conversations by source, ID, timestamp and turn locator. Use in-document anchors; create application links only when their format is verified. Display sanitized evidence as escaped text. Never insert transcript strings into executable scripts, event handlers or raw HTML.

Keep evidence identifiers distinct from proposal IDs. Explain merged or dismissed candidates only as far as useful for deciding. Do not include full transcripts, machine inventories, or implementation logs.

## Verify the generated document

- Confirm that the original template's sample content and all draft placeholders are gone, and that numbers and confidence labels agree with the evidence.
- Check section IDs, table-of-contents links and evidence anchors. Update the template's JavaScript bindings if components or IDs change; remove unused controls and associated code together.
- Open the file in a browser at desktop and narrow mobile widths. Check overflow, readable drafts, navigation, keyboard focus, search and clear behavior, and any disclosures. If retaining search, ensure a match inside a closed disclosure becomes visible.
- Check offline rendering, absence of remote dependencies and browser errors. Confirm content remains available without JavaScript. Do not claim browser verification if tools are unavailable; report that limitation.

The final artifact is a reviewable proposal document. Avoid adding controls that imply edits have been installed or approved when they have not.
