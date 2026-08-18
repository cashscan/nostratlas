# Nostr Atlas

Nostr Atlas is a single-file, client-side web app for exploring Nostr event kinds and learning how Nostr events are structured. It presents a searchable map of documented kinds, links each kind to its NIP documentation, and provides tools for inspecting event JSON and extracting event IDs from links.

## Features

- Search event kinds by number, name, NIP, tag, category, or concept.
- Browse kinds grouped into categories such as Social, Identity, Messaging, Media, Content, Communities, Calendar, Lightning, Development, and Other.
- Open a detail sheet for each kind with its description, status, addressability, related kinds, recognized tags, field explanations, protocol references, and an example event.
- Explain raw Nostr event JSON by displaying its kind, standard fields, tags, and formatted source event.
- Extract a 64-character hexadecimal event ID from supported Nostr links, raw IDs, or event JSON.
- Fetch an extracted event from a WebSocket relay and inspect the returned event.
- Use URL hashes such as `#kind-1` to link directly to a kind.
- Responsive dark-theme interface with keyboard-friendly dialogs, focus restoration, Escape-to-close behavior, and reduced-motion support.
- Uses live kind data from [Nostrbook](https://nostrbook.dev/kinds), with an embedded fallback registry when the live source is unavailable.

## Quick Start

No build step or server is required.

1. Download or clone this project.
2. Open `index-1.html` in a modern browser.
3. Search for a kind, select a card, or use one of the event tools.

For more reliable browser behavior—especially live registry requests and WebSocket relay access—serve the file from a local HTTP server:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/index-1.html
```

## Using the App

### Explore Kinds

Use the main search field to filter the registry. Search examples include:

- `30023`
- `article`
- `reaction`
- `private message`
- `NIP-17`
- `calendar`
- Nostr note links

Select a result to view:

- A plain-language explanation.
- The event’s typical field structure.
- Recognized tags and their meanings.
- Related event kinds.
- A representative JSON event.
- Links to the relevant NIP documentation.

### Explain an Event

Select **Explain a Nostr Event**, paste a JSON event, and choose **Explain event**.

The input must be a JSON object containing a numeric `kind` field. The inspector reports the standard Nostr fields:

- `id`
- `pubkey`
- `created_at`
- `kind`
- `tags`
- `content`
- `sig`

It also explains recognized tags and displays the formatted raw event.

Use **Load example** to populate the inspector with a sample kind 1 text note.

### Extract an Event ID

Select **Extract Event ID** and paste one of the supported inputs:

- A 64-character hexadecimal event ID.
- A supported Nostr note link, including BCH Nostr note links.
- JSON containing an `id` field with a valid hexadecimal event ID.

After extraction, the app can identify an embedded event or fetch the event from a WebSocket relay. The default relay is:

```text
wss://relay.bch.nostr.com
```

You can replace it with another `ws://` or `wss://` relay URL.

## Data Sources

The app starts with an embedded fallback registry so the interface can work without a network request. It then attempts to retrieve and parse kind data from:

```text
https://nostrbook.dev/kinds
```

The source status in the header indicates whether the displayed registry is live or fallback data.

NIP links are generated for:

```text
https://nips.nostr.com/<number>
```

A GitHub NIPs URL is used as a fallback when appropriate.

## Technical Overview

- **Runtime:** Modern browser with JavaScript, `fetch`, `DOMParser`, `WebSocket`, CSS gradients, and standard DOM APIs.
- **Architecture:** One self-contained HTML file containing markup, styles, fallback data, and application logic.
- **Dependencies:** No package manager, framework, bundler, or external JavaScript library is required.
- **Networking:** Optional HTTP fetch for the live kind registry and optional WebSocket connections for event retrieval.
- **State:** Search, overlays, selected kinds, and inspector results are managed in browser memory.
- **Backend:** No backend or persistent database is used.

## Browser Considerations

Opening the file directly with `file://` may restrict cross-origin requests or WebSocket behavior depending on the browser.

Use a local HTTP server when the live registry or relay fetch feature is blocked:

```bash
python3 -m http.server 8000
```

Relay availability, CORS policy, network connectivity, and browser security settings can affect live data and event retrieval.

The fallback kind registry keeps the main explorer usable when the registry request fails.

## Accessibility and Interaction

- Dialog overlays expose modal semantics and `aria-hidden` state.
- Search and form controls have accessible labels or visible context.
- Press `Escape` to close the active overlay.
- Press `/` to focus the search field when another input is not active.
- Focus returns to the element that opened a dialog.
- Reduced-motion preferences are honored through `prefers-reduced-motion`.

## Customization

The primary visual tokens are defined in the `:root` CSS block near the top of the file.

You can adjust:

- Colors.
- Shadows.
- Spacing.
- Typography.
- Panel styling.
- Accent colors.
- Responsive breakpoints.

The fallback registry, category ordering, field explanations, default relay, and source URL are defined in the JavaScript section.

## Limitations

- The app is an educational explorer, not a full Nostr client or event publisher.
- It does not cryptographically verify event signatures.
- It supports a focused set of note-link formats rather than every possible NIP-19 encoding.
- Live registry contents depend on the external Nostrbook source.
- Relay fetching requires a reachable WebSocket relay.
- The requested event must be stored by the selected relay.

## License

No license information is declared in the supplied HTML file. Add a license before redistributing or publishing the project.
