# Basic Timer

## Summary
Basic Timer is a minimal, production-ready countdown timer that runs entirely in your browser. It has no external dependencies and works out of the box on GitHub Pages. The app provides a clean interface with start, pause, and reset controls, keyboard support, a progress indicator, and an audible alert when the timer completes. It also safely handles an optional `?url=` query parameter by exposing a sandboxed preview and a safe external link.

## Setup
No build steps are required.

- Option 1: Open `index.html` directly in a modern browser.
- Option 2: Host the repository on GitHub Pages (or any static host). The app is self-contained.

Browser support: Any modern browser (Chromium, Firefox, Safari, Edge) should work.

## Usage
- Enter the duration (in seconds) in the input field.
- Click Start to begin the countdown.
- Click Pause to pause the timer, and Start again to resume.
- Click Reset to stop and reset the timer to the current input value.
- Use the quick buttons:
  - +1m adds 60 seconds
  - +5m adds 300 seconds
- Keyboard: Spacebar toggles start/pause.
- When finished, the app vibrates (if supported), beeps, and announces “Timer finished”.

Optional query parameter:
- `?url=...`
  - Example: `https://your-domain/path/index.html?url=https://example.com`
  - If present and valid (http/https), the UI will show:
    - A safe “open” link to that URL (opens in a new tab with `rel="noopener noreferrer"`).
    - A “Preview safely” button that loads the URL in a sandboxed iframe with scripts blocked.

Notes:
- The preview is sandboxed to protect you; some sites may not render fully without script permissions.
- Non-http(s) protocols and invalid URLs are ignored for safety.

## Code Explanation
All application code is contained within `index.html`:
- HTML: Semantic structure with accessible labels and ARIA roles. The timer display updates live and includes a progress bar for visual feedback.
- CSS: A minimal, modern UI using system fonts and subtle gradients. The layout is responsive and keyboard-friendly.
- JavaScript:
  - State management: Tracks `initialMs`, `endAt`, `remainingMs`, and `running`.
  - Timing: Uses `Date.now()` to compute remaining time to avoid drift, updating on a small interval plus a `requestAnimationFrame` tick for responsiveness.
  - Controls: Start, Pause, Reset buttons update state and UI. Quick-add buttons (+1m, +5m) modify the duration.
  - Feedback:
    - Document title shows the remaining time while running.
    - Audible alert uses the Web Audio API (no external audio files).
    - Optional speech announcement and vibration for completion.
  - Accessibility: Live regions announce status; spacebar toggles start/pause.
  - Query parameter `?url=` handling:
    - Parses `window.location.search` with `URLSearchParams`.
    - Validates and normalizes with the `URL` constructor.
    - Only allows http/https protocols.
    - Displays a safe external link.
    - Optional sandboxed preview inside an iframe (`sandbox` without scripts; `referrerPolicy="no-referrer"`).

Safety considerations:
- No external dependencies or inline remote scripts.
- The `?url=` handler blocks non-http(s) protocols and invalid URLs.
- Iframe preview is sandboxed to mitigate risks; scripts are not allowed.
- All user-provided values are inserted via safe DOM properties, not `innerHTML`.

## License
This project is released under the MIT License. You are free to use, modify, and distribute it with attribution. See the LICENSE file if present, or treat this statement as the license grant.