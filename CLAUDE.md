# CLAUDE.md — img2css

This file provides guidance for Claude Code (and other AI coding assistants) working in this repository.

## Project Overview

**img2css** is a React web app that converts any image into a pure-CSS representation using only `box-shadow` properties on a single `<div>`. It is a creative/demo tool — not a production utility — live at [javier.xyz/img2css](https://javier.xyz/img2css).

Upstream repo: https://github.com/javierbyte/img2css  
This fork: https://github.com/Mazik71/img2css

## Tech Stack

- **React** (Create React App) — UI framework
- **JavaScript (ES6+)** — no TypeScript
- **CSS Modules / plain CSS** — styling
- **ESLint + Prettier** — linting and formatting (see `.eslintrc.js` and `.prettierrc`)

## Development Commands

```bash
# Install dependencies
npm install

# Start local dev server (hot reload on http://localhost:3000)
npm start

# Run linter
npm run lint

# Build for production (output to /build)
npm run build
```

## Repository Structure

```
img2css/
├── public/           # Static assets and index.html
├── src/
│   ├── App.js        # Root component — handles image upload and state
│   ├── components/   # Reusable UI components
│   ├── utils/        # Core image-to-CSS conversion logic
│   └── index.js      # Entry point
├── docs-assets/      # Screenshots and documentation images
├── .eslintrc.js      # ESLint config
├── .prettierrc       # Prettier config
└── CHANGELOG.md      # Version history
```

## Core Algorithm

The conversion lives in `src/utils/`. The key steps are:

1. Draw the uploaded image onto an off-screen `<canvas>`.
2. Read pixel data via `canvas.getContext('2d').getImageData()`.
3. Downsample to a target resolution (user-controlled pixel size).
4. Build a `box-shadow` CSS string — one shadow per pixel, offset by `(x, y)` with the sampled `rgba()` color.
5. Apply the result to a 1×1 `<div>`, which renders the image purely in CSS.

## Key Constraints & Notes

- **No backend** — everything runs client-side in the browser.
- **Performance** — large images or small pixel sizes generate very long `box-shadow` strings and can freeze the browser tab. Warn users before processing high-resolution inputs.
- **Browser support** — relies on Canvas API and CSS `box-shadow`; avoid features not broadly supported.
- **Do not change the core pixel-sampling algorithm** without verifying visual output matches the original — regression is hard to unit-test automatically.
- The app intentionally has **no routing** — it is a single-page, single-view tool.

## Coding Conventions

- Use **functional React components** and hooks only (no class components).
- Follow existing ESLint rules — run `npm run lint` before committing.
- Keep components small and single-purpose.
- Prefer descriptive variable names for pixel/color math (e.g. `pixelSize`, `rgbaColor`) for readability.

## Testing

There are currently no automated tests. When adding logic to `src/utils/`, consider adding simple unit tests with Jest (`npm test`) to verify pixel-sampling correctness with known inputs.
