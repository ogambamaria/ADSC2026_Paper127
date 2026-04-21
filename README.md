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

All three contributors share a single **Zotero Group Library**. References are added to the group collection by any contributor, but `references/references.bib` is updated and committed following a controlled manual export process. This prevents one contributor's export from silently overwriting another's additions.

IEEE uses **numeric citations** rendered as `[1]`, `[2]`, etc. Citation order in the compiled document is determined by order of appearance in the text. The bibliography style is set in `main.tex` and must not be changed:

```latex
\bibliographystyle{IEEEtran}
\bibliography{IEEEabrv,references/references}
```

---

### One-Time Setup

#### 1. Join the Zotero Group Library

All contributors must have a Zotero account and be members of the shared group. Accept the invitation at [https://www.zotero.org/groups/6493800/adsc2026](https://www.zotero.org/groups/6493800/adsc2026). The group library will appear in the left panel of Zotero under **Group Libraries**.

#### 2. Install Better BibTeX

Download and install from [https://retorque.re/zotero-better-bibtex/installation/](https://retorque.re/zotero-better-bibtex/installation/).

Better BibTeX (BBT) assigns stable, human-readable citation keys to each item (e.g., `smith2023climate`) and handles export to `.bib` format. BBT is compatible with Zotero 8 and above.

#### 3. Do Not Configure AutoExport

Do not use BBT's AutoExport (`Keep updated`) on this project. With multiple contributors on different machines, AutoExport causes each machine to independently overwrite `references/references.bib` on every library change, producing conflicts. Use the manual export process below instead.

---

### Adding References

Add references to the **shared group collection** only — not your personal Zotero library. Items added to personal libraries are not visible to other contributors and will not appear in the shared export.

References can be added via:
- The Zotero browser connector
- DOI or ISBN lookup (`File` → `Import by Identifier`)
- Manual entry

---

### Updating `references/references.bib`

Run this sequence every time you add new references and need to commit them to the repository:

```bash
git checkout main
git pull origin main
git checkout -b refs/add-your-description
```

Then export from Zotero:

1. Right-click the group collection in Zotero → `Export Collection`
2. Format: `Better BibTeX`
3. Leave `Keep updated` unchecked
4. Set the export path to `references/references.bib` in the repository root

Then commit and open a PR:

```bash
git add references/references.bib
git commit -m "refs: add sources for [section name]"
git push origin refs/add-your-description
```

Open a pull request as normal. Do not update `references/references.bib` directly on `main`.

> Before exporting, confirm Zotero has fully synced the group library (`Edit` → `Sync` or the sync icon in the toolbar). Exporting before a sync completes will produce an incomplete `.bib` file.

---

### Inserting Citations in VS Code

With Zotero running and the VS Code Zotero extension installed:

1. Position cursor at the citation point in your `.tex` file.
2. Open the Command Palette: `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS).
3. Run `Zotero: Pick Citation`.
4. Search by title, author, or year.
5. Select the item — the citation key is inserted as `\cite{key}`.

> Zotero must be open and running for the citation picker to function. The item must exist in the group collection, not only in a personal library, to guarantee it is present in `references/references.bib`.

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
