# TFO Designer — Triplex DNA Analysis Platform

## Overview

A comprehensive scientific web platform for Triplex DNA analysis and ligand interaction studies.
Built as a pnpm monorepo with a React/Vite frontend and an Express API backend.

## Main Platform: TFO Designer (`artifacts/tfo-app`)

A 9-step workflow for triplex DNA research:
1. **DNA Input** — manual paste or NCBI GenBank/Nucleotide search by gene name or accession
2. **Triplex Target Detection** — polypurine scanning for TTS sites (Triplexator-inspired)
3. **TFO Design** — sliding-window TFO generation with A/B/C/D variants, GC%, Tm, score ranking
4. **Ligand Input** — SMILES notation, file upload (SDF/MOL/PDB), or library selection
5. **PDB Structure** — RCSB PDB retrieval for known DNA-triplex structures
6. **Docking** — AutoDock Vina-inspired binding energy estimation
7. **MD Simulation** — GROMACS-inspired molecular dynamics (RMSD, energy, stability)
8. **AI Prediction** — ML-based triplex probability, binding strength, TFO ranking
9. **Results Dashboard** — 3D viewer (3Dmol.js), animation, charts, export

## Stack

- **Monorepo tool**: pnpm workspaces
- **Frontend**: React 19 + Vite 7 + TypeScript (artifacts/tfo-app)
- **UI**: Tailwind CSS v4 + Radix UI (shadcn/ui components)
- **Charting**: Recharts
- **Animation**: Framer Motion
- **3D Visualization**: 3Dmol.js (CDN)
- **State**: React Context + useReducer (workflow state machine)
- **Routing**: Wouter
- **API Server**: Express 5 (artifacts/api-server)
- **Build**: esbuild (API server), Vite (frontend)

## Key Files

- `artifacts/tfo-app/src/lib/tfoEngine.ts` — TFO scanning algorithms (TypeScript port of Python engine)
- `artifacts/tfo-app/src/lib/scoring.ts` — GC%, Tm, stability scoring
- `artifacts/tfo-app/src/lib/dockingEngine.ts` — simulated docking + MD
- `artifacts/tfo-app/src/lib/workflowContext.tsx` — global state management
- `artifacts/tfo-app/src/lib/ncbiApi.ts` — NCBI and RCSB PDB API integration
- `artifacts/tfo-app/src/components/steps/` — 8 workflow step components
- `artifacts/tfo-app/src/components/visualization/` — 3D viewer + animation
- `artifacts/tfo-app/src/components/results/Dashboard.tsx` — results page

## Scientific Integrations

- NCBI GenBank / Nucleotide — real-time sequence retrieval
- RCSB PDB — 3D structure fetch (DNA triplex entries)
- Triplexator / LongTarget / TDF — algorithm inspiration
- AutoDock Vina — docking methodology
- GROMACS — MD simulation methodology
- RDKit — ligand property computation

## Structure

```text
artifacts-monorepo/
├── artifacts/              # Deployable applications
│   └── api-server/         # Express API server
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── tfo-designer/           # Python Streamlit TFO Designer app
│   ├── .streamlit/         # Streamlit config (port 5000)
│   └── app.py              # Main Streamlit application
├── scripts/                # Utility scripts (single workspace package)
├── pnpm-workspace.yaml     # pnpm workspace (artifacts/*, lib/*, lib/integrations/*, scripts)
├── tsconfig.base.json      # Shared TS options (composite, bundler resolution, es2022)
├── tsconfig.json           # Root TS project references
└── package.json            # Root package with hoisted devDeps
```

## TFO Designer (Python/Streamlit)

A bioinformatics web app for designing Triplex Forming Oligonucleotides.

- **Entry**: `tfo-designer/app.py`
- **Run**: `streamlit run tfo-designer/app.py --server.port 5000`
- **Workflow**: "Start application" (port 5000, webview)
- **Features**:
  - Fetches DNA sequences from NCBI using accession numbers (E-utilities API)
  - Parses FASTA format via Biopython
  - Sliding window scan (15–30 bp, configurable) for polypurine (A/G) regions
  - Generates TFO sequences using pyrimidine parallel motif (A→T, G→C)
  - Calculates GC content and approximate Tm (Wallace rule / simplified NN)
  - Results table with position, target sequence, TFO sequence, GC%, Tm
  - CSV download of results
- **Dependencies**: streamlit, biopython, requests, pandas

## TypeScript & Composite Projects

Every package extends `tsconfig.base.json` which sets `composite: true`. The root `tsconfig.json` lists all packages as project references.
