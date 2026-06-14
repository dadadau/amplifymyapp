# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## Project overview

`amplifymyapp` is a single-page React application bootstrapped with
[Create React App](https://github.com/facebook/create-react-app) (CRA). It is
currently the unmodified CRA starter template — a minimal baseline ready to be
built out into a real app.

- **Framework:** React 17 (`react` / `react-dom` `^17.0.2`)
- **Build tooling:** `react-scripts` `5.0.0` (webpack/Babel/ESLint/Jest are all
  wrapped by CRA; configs are not ejected)
- **Testing:** Jest + React Testing Library (`@testing-library/react`,
  `@testing-library/jest-dom`, `@testing-library/user-event`)
- **Package manager:** npm (a `package-lock.json` is committed)
- **Node module system:** ES modules (`import` / `export`) compiled by Babel

## Repository structure

```
.
├── public/             # Static assets served as-is; not processed by webpack
│   ├── index.html      # HTML template; React mounts into <div id="root">
│   ├── manifest.json   # PWA manifest
│   ├── robots.txt
│   └── favicon.ico, logo192.png, logo512.png
├── src/                # Application source (all app code lives here)
│   ├── index.js        # Entry point: renders <App> into #root (StrictMode)
│   ├── App.js          # Root component
│   ├── App.css         # Styles for App
│   ├── App.test.js     # Example test for App
│   ├── index.css       # Global styles
│   ├── logo.svg
│   ├── reportWebVitals.js  # Optional performance measurement hook
│   └── setupTests.js   # Jest setup; imports jest-dom matchers
├── package.json
├── package-lock.json
└── README.md           # Default CRA documentation
```

Build output goes to `build/` (gitignored). Dependencies live in `node_modules/`
(gitignored).

## Development workflow

Install dependencies first (CI/fresh clones):

```bash
npm install
```

Common commands:

| Command           | What it does                                                        |
| ----------------- | ------------------------------------------------------------------- |
| `npm start`       | Run the dev server at http://localhost:3000 with hot reload         |
| `npm test`        | Run Jest in interactive watch mode (see CI note below)              |
| `npm run build`   | Produce an optimized production build in `build/`                    |
| `npm run eject`   | Eject CRA config — **one-way, irreversible; avoid unless required** |

### Running tests non-interactively

`npm test` defaults to watch mode. For a single CI-style run:

```bash
CI=true npm test
# or
npx react-scripts test --watchAll=false
```

## Conventions

- **Component files:** Function components in `.js` files using JSX. The root
  component is `App` in `src/App.js`; new components go under `src/`.
- **Styling:** Plain CSS imported directly into components
  (`import './App.css'`). Existing classes use PascalCase-ish names like
  `App-header`, `App-link`. Follow the existing pattern unless introducing a
  deliberate new styling approach.
- **Tests:** Co-locate tests next to the code as `*.test.js` and write them with
  React Testing Library — query by user-visible text/roles (see
  `src/App.test.js`), not implementation details. `setupTests.js` loads
  `@testing-library/jest-dom` matchers (e.g. `toBeInTheDocument`).
- **Linting:** ESLint runs through `react-scripts` with the `react-app` and
  `react-app/jest` configs (declared in `package.json`). Lint warnings surface in
  the dev server console and the build output — keep the build warning-free.
- **Strict mode:** The app is wrapped in `<React.StrictMode>` in `src/index.js`;
  keep components side-effect-safe under double-invocation.
- **Static assets:** Put files that must be referenced by URL or kept unprocessed
  in `public/`. Import asset files (images, etc.) into `src/` modules to let
  webpack fingerprint them.

## Notes for AI assistants

- This is a fresh CRA template; there is no custom routing, state management, API
  layer, or CI workflow yet. Don't assume infrastructure that isn't present —
  check before referencing it.
- Configs (webpack, Babel, Jest) are **not** ejected. Customize via supported CRA
  mechanisms (e.g. `package.json` fields, `.env` files) rather than editing
  tool configs directly. Do not run `npm run eject` unless explicitly asked.
- React 17 is in use — the entry point uses `ReactDOM.render` (the React 17 API),
  not React 18's `createRoot`. Match the installed version when adding code, or
  upgrade deliberately.
- Run `CI=true npm test` and `npm run build` to validate changes before
  committing.
- Keep dependency changes in sync: commit `package-lock.json` alongside
  `package.json` edits.
```
