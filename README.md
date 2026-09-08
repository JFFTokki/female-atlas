# Human Atlas

An interactive 3D anatomy explorer built with React, Three.js, and shadcn/ui. Explore selectable male and female reference models with a continuous assembled-to-exploded view. The male BodyParts3D collection contains **2,234 meshes** and **3,432 named concepts**; the female Human Reference Atlas collection contains **888 meshes** and **1,073 selectable source nodes**.

**[Explore the live demo](https://human-atlas-seven.vercel.app)**

## Explore

- Choose male or female anatomy; geometry, catalogue, and available systems change together.
- Orbit, zoom, and select structures directly on the body.
- Toggle individual systems or use skeleton and organ presets.
- Move from assembled anatomy to a spaced inventory of every visible piece.
- Search anatomical names and source identifiers.
- Isolate a selected structure and read its details.
- Use compact controls and detail panels on mobile.

## Run locally

Requires Node.js 22.13 or newer. No API keys or accounts are needed.

```sh
npm ci
npm run dev
```

Open http://localhost:3016. To build the static site, run `npm run build`; the output is in `dist/`.

## Validate

```sh
npm run check
node scripts/validate-atlas.mjs
node scripts/validate-atlas.mjs atlas-female.json
node scripts/validate-interactions.mjs
npm run build
```

Validation covers both reference models, mesh buffers, names and concept membership, nonoverlapping exploded layouts at desktop and mobile aspect ratios, search and inspection contracts, and tap-versus-drag handling. Browser interaction checks have exercised selection, system controls, search, isolation, rotation, and 390×844, 320×568, and 844×390 layouts. Phone controls stay clear of the exploded inventory, and isolated structures fit the space above or beside the detail panel. Physical-device performance and real multitouch hardware have not been tested.

## Anatomy data

The viewer uses **BodyParts3D 4.0** adult male anatomy and **Human Reference Atlas united-female v1.5**, both licensed **CC BY 4.0**. The female collection includes reproductive anatomy, whole-body surface, and selected organs; skeleton and muscle coverage is partial. Eight pregnancy reference pieces are hidden by default in a separate layer. Neither collection represents every human structure or variation. Individual source meshes are distinct from named concepts, which may group multiple meshes.

Geometry is simplified for browser performance while retaining every source mesh. The packaged male model contains 2,288,268 triangles and downloads approximately 33 MB of compressed geometry; the female model contains 1,810,038 triangles and downloads approximately 23.6 MB. Together they contain 4,098,306 triangles and require approximately 56.6 MB of compressed geometry. Full credits, source links, and adaptation details are in [ATTRIBUTION.md](public/ATTRIBUTION.md).

This is an educational explorer, not a diagnostic or surgical tool.

## How it works

Geometry is merged into batches. Per-structure GPU textures control translation, visibility, and selection, while component geometry supports accurate picking. Exploded layouts pack only the visible pieces. Rendering updates when the scene changes; orbit controls remain responsive without thousands of separate draw calls.

The optional WebMCP tools expose anatomy search and inspection in compatible browsers. The visible interface works without them.

## Rebuilding geometry

The repository includes browser-ready geometry. Rebuilding it is optional: obtain the official BodyParts3D OBJ archive and English metadata tables, prepare the joined concepts and display-system mappings, run `scripts/convert-anatomy.py`, then `node scripts/optimize-anatomy.mjs` and `node scripts/compress-models.mjs`. To rebuild the female collection, obtain the source HRA GLB linked in [ATTRIBUTION.md](public/ATTRIBUTION.md) and prepare a `SOURCE_PARTS.json` file with a `parts` array keyed by GLB `nodeIndex`; each record must supply `id`, `name`, `system`, and `parents`, with optional `sourceLabel` and `ontologyId`. Then run `python3 scripts/convert-female.py SOURCE.glb SOURCE_PARTS.json`, `node scripts/optimize-anatomy.mjs atlas-female.json`, and `node scripts/compress-models.mjs`. The prepared `SOURCE_PARTS.json` is not included in this repository. Simplification uses a 0.2% relative error limit per structure.

## Deploy

Import this repository into Vercel as a Vite project. The included `vercel.json` configures `npm ci`, `npm run build`, and the `dist` output directory. It can also be served by a static host.

## License

Original application code is released under the [MIT License](LICENSE). **The anatomy data has its own CC BY 4.0 license**; preserve the attribution when redistributing it. Third-party dependencies retain their respective licenses.

Issues and pull requests are welcome. Please include reproduction steps and browser/device details for interaction problems.
