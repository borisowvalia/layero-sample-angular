# layero-sample-angular

Angular CLI fixture (latest, with routing, SSR disabled) for [Layero](https://layero.ru).

## Quick facts

- **Framework**: Angular (latest CLI), routing enabled, **SSR disabled** (`--ssr=false`)
- **Build command**: `npm run build`
- **Output directory**: `dist/layero-sample-angular/browser/`
- **Node**: 20+

## What it demonstrates

Angular **17+ changed the default output layout**: the build now writes to `dist/<project>/browser/` (and `dist/<project>/server/` if SSR is on). Layero **does not have an Angular detector** today, so it falls through to Generic and looks for `dist/`. If you set `output_dir: dist`, the upload contains an empty `<project>/` subfolder — broken site.

After fix A1 (`builder/src/frameworks/angular.py`):
- Read `angular.json` → find first key under `projects` (or `defaultProject` for older versions).
- Set `output_dir = dist/<project>/browser`.
- Set `build_cmd = npm run build -- --configuration production`.

## Deploy on Layero

1. Add the project, point at this repo.
2. **Manually set** `output_dir: dist/layero-sample-angular/browser` (until detector A1 ships).
3. `build_cmd: npm run build` is correct by default.

## Local verify

```bash
npm install
npm run build
ls dist/layero-sample-angular/browser/
npx serve dist/layero-sample-angular/browser
```

## Note on routing

Angular Router uses HTML5 History API by default. SPA fallback (`error_page 404 = /index.html`) on Layero edge already handles this. No additional config needed once `output_dir` is correct.
