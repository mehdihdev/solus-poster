# Solus conference poster

Full-screen animated poster for a monitor at the booth. Static site — no build step.

```
index.html      the poster
solus-qr.png    QR code → solusforge.com
vercel.json     static config
reference/      the original Claude Design source (Solus Poster.dc.html + support.js)
```

## Preview locally

```sh
python3 -m http.server 8080
# open http://localhost:8080
```

## Deploy

```sh
npx vercel --prod
```

Accept the defaults (framework: Other, no build command, output: the project root).

## Running it at the conference

- Open the deployed URL in a browser and press **F11** (Windows/Linux) or **⌃⌘F** (macOS) for full screen.
- The layout is viewport-fixed with `overflow:hidden`, so it fills any screen without scrollbars.
- Turn off the display sleep / screensaver on the presenting machine.
- Everything animates in CSS plus one small timer; it runs indefinitely with no memory growth.

## Notes on the implementation

`reference/Solus Poster.dc.html` is the Claude Design source. It depends on `support.js`, a
React + Babel runtime that the canvas loads from unpkg at page load. `index.html` is the same
markup, CSS and SVG verbatim, with the one dynamic binding — the rotating caption under the
tagline — reimplemented in ~15 lines of vanilla JS that matches the original `DCLogic`
component exactly (fade out, swap line after 650 ms, fade in, every 4 s). That removes three
third-party script downloads and in-browser transpilation from the critical path, which matters
on conference Wi-Fi.

Fonts (Inter, JetBrains Mono) still load from Google Fonts, as in the design. If the venue's
network is unreliable, the page falls back to `system-ui` and the default monospace.
