# WorldMonitor Copilot Instructions

## Monorepo Architecture
This is a TypeScript SPA (Vite + Preact) with Vercel Edge API endpoints, a Tauri desktop app, and a Railway relay service.

## Critical Rules
1. **NO REACT/JSX**: The frontend is vanilla TypeScript using class-based components extending `Panel`. Do not suggest React hooks, components, or JSX.
2. **EDGE FUNCTION ISOLATION**: Files in `api/*.js` are Vercel Edge Functions. They CANNOT import from `../src/` or `../server/`.
3. **PROTO-FIRST RPC**: API endpoints use the `sebuf` framework. Always start by editing `proto/worldmonitor/` and running `make generate`.
4. **CACHING**: All RPC handlers must use `cachedFetchJson()` from `server/_shared/redis.ts`.
5. **FETCH BANNED**: Never use `fetch.bind(globalThis)`. Use `(...args) => globalThis.fetch(...args)`.

## Adding Endpoints
1. Define proto message in `proto/worldmonitor/<domain>/`.
2. Add RPC with `(sebuf.http.config)`.
3. Run `make generate`.
4. Create handler in `server/worldmonitor/<domain>/`.
5. Use `cachedFetchJson()` for upstream fetches.
6. Wire handler in domain's `handler.ts`.

## Adding UI Panels
1. Create `src/components/MyPanel.ts` extending `Panel`.
2. Register in `src/config/panels.ts`.
3. Add to variant configs in `src/config/variants/`.
4. Wire data loading in `src/app/data-loader.ts`.

## Testing & Validation
- Run `npm run typecheck:all` after making changes.
- Edge functions are tested via `tests/edge-functions.test.mjs`.
- Always check for proto freshness via `make generate`.
