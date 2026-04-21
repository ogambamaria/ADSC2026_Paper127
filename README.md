# LaTeX Document Management — IEEE Template

This repository uses the official **IEEE conference/journal LaTeX template**. Do not substitute or modify the template class file (`IEEEtran.cls`). All formatting — column layout, font sizes, heading styles, caption formatting, and bibliography style — is governed by the template and must not be overridden with manual formatting commands.

---

## Prerequisites

The following tools must be installed before contributing to or building documents in this repository.

---

### 1. Visual Studio Code

Download and install from [https://code.visualstudio.com](https://code.visualstudio.com).

VS Code is the required editor for this project. All configuration, extensions, and workflow instructions assume VS Code as the working environment.

---

### 2. LaTeX Distribution

A local LaTeX distribution must be installed on your machine:

- **Windows / Linux**: [TeX Live](https://tug.org/texlive/)
- **macOS**: [MacTeX](https://tug.org/mactex/)

---

### 3. VS Code Extensions

Install the following extensions from the VS Code Marketplace:

#### LaTeX Workshop
**ID**: `James-Yu.latex-workshop`

Provides LaTeX compilation, PDF preview, syntax highlighting, and IntelliSense directly in VS Code.

#### Zotero for VS Code (Citation Picker)
**ID**: `mblode.zotero`  

Enables citation insertion from your Zotero library directly into `.tex` files via a searchable picker (`Ctrl+Shift+P` → `Zotero: Pick Citation`). Requires Zotero to be running locally with the Better BibTeX extension active.

---

### 4. Zotero

Download and install from [https://www.zotero.org](https://www.zotero.org).

Zotero is the reference manager used across this project. All references are managed in Zotero and exported automatically to the repository's `.bib` file.

---

### 5. Better BibTeX for Zotero

Download and install from [https://retorque.re/zotero-better-bibtex/installation/](https://retorque.re/zotero-better-bibtex/installation/).

Better BibTeX (BBT) handles the automatic, continuous export of your Zotero library to a `.bib` file. This is the core mechanism connecting Zotero to the LaTeX build.

---

## Reference Workflow: Zotero → BibTeX → LaTeX

### Overview

Each contributor maintains a **personal `.bib` file** that auto-exports from their local Zotero library. This eliminates merge conflicts during active writing. When work is ready for integration, references are promoted to the shared `references/references.bib` via a manual export from the group library.

IEEE uses numeric citations rendered as `[1]`, `[2]`, etc. The bibliography style is fixed in `main.tex` and must not be changed:

```latex
\bibliographystyle{IEEEtran}
\bibliography{IEEEabrv,references/references}
```

During the personal workflow phase, each contributor temporarily points their local build at their own `.bib` file. The `main.tex` on `main` branch always references `references/references.bib`.

---

### One-Time Setup

#### 1. Join the Zotero Group Library

Accept the invitation at [https://www.zotero.org/groups/6493800/adsc2026](https://www.zotero.org/groups/6493800/adsc2026). The group library appears in the Zotero left panel under **Group Libraries**.

#### 2. Install Better BibTeX

Download and install from [https://retorque.re/zotero-better-bibtex/installation/](https://retorque.re/zotero-better-bibtex/installation/).

BBT assigns stable citation keys (e.g., `smith2023climate`) and handles auto-export to `.bib` format.

---

#### Configuring AutoExport in BBT

1. In Zotero, right-click the collection → `Export Collection`.
2. Format: `Better BibTeX`.
3. Check `Keep updated`.
4. Set the export path to your named file, e.g., `references/maria.bib`, inside the repository root.

BBT will now update your file automatically whenever your local Zotero library changes. No manual export step is required during writing.

#### Local build configuration

On your working branch, temporarily adjust the bibliography command in your local `main.tex` to include your personal file:

```latex
\bibliography{IEEEabrv,references/maria,references/references}
```

LaTeX resolves citation keys across all listed `.bib` files. Keys present in your personal file but not yet in `references/references.bib` will resolve correctly in local builds.

Do not commit this change. Revert `main.tex` to the canonical bibliography command before opening a pull request.

---

### Adding References During Writing

Add references to your **personal Zotero collection**. BBT exports them automatically to your `[firstname].bib`. They are immediately available for citation in your local build.

Do not add to the group collection during active writing. The group collection is the source for the shared file and should only receive entries that are ready for integration.

---

### Promoting References to the Shared File

Run this sequence when a section is ready for PR or when coordinating a merge with other contributors.

#### Step 1 — Add your references to the group collection

In Zotero, drag the relevant items from your personal collection into the group collection. Verify sync has completed (`Edit` → `Sync` or the toolbar sync icon) before proceeding.

#### Step 2 — Export from the group collection

1. Right-click the group collection → `Export Collection`.
2. Format: `Better BibTeX`.
3. Leave `Keep updated` unchecked.
4. Set the export path to `references/references.bib` in the repository root.

#### Step 3 — Commit on a dedicated branch

```bash
git checkout main
git pull origin main
git checkout -b refs/promote-[your-description]
git add references/references.bib
git commit -m "refs: promote references for [section name]"
git push origin refs/promote-[your-description]
```

Open a pull request as normal. One reviewer must approve before merge.

> Confirm all contributors have synced the group library before exporting. Exporting before a sync completes produces an incomplete `.bib` file.

---

### Inserting Citations in VS Code

With Zotero running and the VS Code Zotero extension installed:

1. Position cursor at the citation point in your `.tex` file.
2. Open the Command Palette: `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS).
3. Run `Zotero: Pick Citation`.
4. Search by title, author, or year.
5. Select the item — the citation key is inserted as `\cite{key}`.

The item must exist in your personal collection (for local builds) or the group collection (for shared builds). Items in personal Zotero libraries not mirrored in either place will not resolve in a collaborator's build.

---

### Phase Transition: Switch to Group-Only Workflow

When the draft stabilises and active per-contributor writing ends, the project switches to group-only reference management:

1. All contributors promote remaining personal references to the group collection and verify sync.
2. One contributor performs a final export from the group collection to `references/references.bib` and opens a PR.
3. After merge, the `gitignore` rule for `references/refs-*.bib` can remain in place — personal files are simply no longer updated or used.
4. Going forward, all new references are added directly to the group collection and promoted via the manual export process above.

The bibliography command in `main.tex` on `main` requires no changes — it already points to `references/references.bib`.

---

## Git Workflow and Version Control

### Branch Strategy

All work must be done on a **dedicated branch**. Direct commits to `main` are not permitted.

### Committing Work

```bash
git add .
git commit -m "Descriptive message of what changed"
git push origin your-branch-name
```

### Pull Requests

A **pull request (PR) must be opened** before any branch is merged into `main`. Direct pushes to `main` are blocked.

PR requirements:

- The branch must be up to date with `main` before opening a PR.
- The PR description must summarise the changes made.
- At least one reviewer must approve before merging.
- The document must compile without errors before merge is approved.

To open a PR:

1. Push your branch to the remote.
2. Navigate to the repository on GitHub.
3. Click `Compare & pull request`.
4. Fill in the description and assign a reviewer.
5. Do not merge your own PR without a review (except in solo projects with a documented reason).

### Keeping Your Branch Current

```bash
git checkout main
git pull origin main
git checkout your-branch-name
git merge main
```

Resolve any merge conflicts before pushing.

---

## Project Structure

```
.
├── main.tex                  # Root LaTeX document (IEEE template entry point)
├── IEEEtran.cls              # IEEE class file — do not modify
├── IEEEabrv.bib              # IEEE journal title abbreviations — do not modify
├── references/
│   └── references.bib        # Auto-exported by Zotero/Better BibTeX
├── sections/                 # Individual .tex section files
├── figures/                  # All images and generated plots
├── tables/                   # Standalone table .tex files (optional)
├── output/                   # Compiled PDFs (gitignored)
└── .gitignore
```

> `output/` must be listed in `.gitignore`. Compiled PDFs are build artefacts, not source files.

> `IEEEtran.cls` and `IEEEabrv.bib` are part of the official IEEE template distribution. Neither file should be edited. `IEEEabrv.bib` provides standardised abbreviated journal names required by `IEEEtran` bibliography style — reference it in your bibliography command if abbreviated journal names are needed:

```latex
\bibliography{IEEEabrv,references/references}
```

---

## Compilation

Open the project in VS Code. LaTeX Workshop compiles automatically on save, or manually via:

- `Ctrl+Alt+B` (Windows/Linux)
- `Cmd+Option+B` (macOS)

The compiled PDF opens in the side panel. If compilation fails, check the LaTeX Workshop output log in the terminal panel.

### Compilation Order

IEEE documents with a bibliography require the following compilation sequence to resolve all cross-references and citation numbers correctly:

```
pdflatex → bibtex → pdflatex → pdflatex
```

LaTeX Workshop handles this automatically when configured with the `latexmk` recipe (default). If references show as `[?]` in the output, run a full build rather than a single-pass compile.

### IEEE-Specific Constraints

The following are hard constraints imposed by the IEEE template and editorial requirements. Non-compliant submissions are rejected without review.

- **Two-column layout** is enforced by `IEEEtran.cls`. Do not insert single-column overrides except for wide figures or tables using the `figure*` / `table*` environments.
- **No custom fonts**. The template defines the font stack. Do not add `fontspec`, `setmainfont`, or equivalent commands.
- **No manual spacing adjustments** (`\vspace`, `\hspace`, `\enlargethispage`) used to manipulate layout for page count.
- **Figure captions** go below the figure. Table captions go above the table. This is enforced by IEEE style, not the class file — apply it consistently.
- **Page limit** is submission-specific. Check the call for papers. The compiled PDF page count on `main` must comply before a PR is merged.
- **Author information** in `main.tex` must be removed or anonymised for double-blind submissions. Confirm the submission type before final build.
