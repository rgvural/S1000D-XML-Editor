# S1000D Editor — User Guide

*Version 0.45.4*

Set up a project from nothing, edit S1000D data modules visually, validate them against XSD and BREX, and produce a PDF from one module or from a whole publication.

## Contents

- [Installation](#installation)
  - [Running from source](#running-from-source)
- [Interface](#interface)
- [The ribbon and command search](#the-ribbon-and-command-search)
  - [Command search — `Ctrl+K`](#command-search--ctrlk)
- [Documenting a new system](#documenting-a-new-system)
- [Opening and saving files](#opening-and-saving-files)
- [Visual (WYSIWYG) editing](#visual-wysiwyg-editing)
  - [Editing text](#editing-text)
  - [Adding a figure](#adding-a-figure)
  - [Text formatting](#text-formatting)
  - [Untouchable items (atoms)](#untouchable-items-atoms)
  - [Block operations](#block-operations)
  - [Tag view](#tag-view)
- [XML mode](#xml-mode)
- [Validation — ✓ Validate](#validation---validate)
- [Module properties — 📋 Properties](#module-properties---properties)
- [Publication style — 🎨 Style](#publication-style---style)
- [Bulk import — 📊 (Home tab)](#bulk-import---home-tab)
- [The S-Series bridge — 🔗](#the-s-series-bridge--)
  - [S2000M → IPD](#s2000m--ipd)
  - [S4000P → Scheduled maintenance](#s4000p--scheduled-maintenance)
  - [S3000L → Procedure skeleton](#s3000l--procedure-skeleton)
  - [Mapping profile](#mapping-profile)
  - [The other direction: the S3000L catalogue](#the-other-direction-the-s3000l-catalogue)
- [Translation — 🌐 Translate (Publication tab)](#translation---translate-publication-tab)
- [Producing a PDF](#producing-a-pdf)
  - [A single module](#a-single-module)
  - [A publication (Publication Module)](#a-publication-publication-module)
  - [Page view](#page-view)
- [The S1000D issue — Issue 6 and Issue 7](#the-s1000d-issue--issue-6-and-issue-7)
- [Project profile — 🎛](#project-profile--)
- [Business rules — ⚖](#business-rules--)
  - [Decision points (BRDP)](#decision-points-brdp)
- [Front matter reports — 📑](#front-matter-reports--)
- [Supported content](#supported-content)
- [Common situations](#common-situations)
- [Release history](#release-history)

## Installation

| Platform | Package | Note |
|----|----|----|
| Windows (x64) | `S1000D Editor Setup 0.45.4.exe` | Setup wizard; the directory can be chosen |

### Running from source

    npm install
    npm run dev        # development (live reload)
    npm start          # build + run
    npm run dist:win   # build the Windows installer .exe

## Interface

*\[Screen diagram: Application window layout\]*

The structure bar gains buttons according to the document type: in an IPD, ＋ Part row / ⇪ Import BOM; in a BREX module, ⚖ BREX rules.

> **Reaching this guide:** the **? Help** button on the **View** tab, or the **Help → User Guide** menu (`F1`). The guide is embedded in the application; no internet is needed. File operations also have shortcuts: `Ctrl+O` open, `Ctrl+S` save, `Ctrl+P` PDF.

- **Files** — every XML in the open module's directory; one click to switch.
- **Document Map** — the document's element tree. Click a node and that block is selected in the preview and scrolled into view. Right-click for *Change…* (edit the attributes and, if it has no child elements, the text) and *Delete* (after confirmation, removes the element with its subtree; the root element cannot be deleted). Both can be undone.
- **Panel widths** — drag the splitter between the sidebar and the content; in XML mode there is a splitter between the editor and the preview too. Double click restores the default, arrow keys fine-tune. The settings are kept.
- **Element path** — the hierarchy of the selected block; click a segment to select the parent element.

## The ribbon and command search

Commands are divided into tabs by subject. Click a tab and only that group's commands are shown; the name of each set is printed under it.

*\[Screen diagram: Ribbon tabs\]*

| Tab | Contents |
|----|----|
| **Home** | New Project, Open/Save/PDF, Undo/Delete, formatting, project profile, module properties |
| **Insert** | Paragraph, section, step, list item, warning/caution/note, figure |
| **Module** | The module types that can be added (according to the project profile) |
| **Publication** | Publication style, front matter reports, translation, produce PDF |
| **BREX** | XSD + BREX validation, the rule editor, business rule decisions |
| **View** | Visual–XML mode, tags, sidebar, command search, interface language, the guide |

**Context tabs:** a *Parts Catalogue* tab appears by itself in an IPD module, *BREX Rules* in a BREX module and *Business Rules* in a business rules module; it disappears when the file is closed.

**Interface language:** English by default. Switch to Turkish under **View → Interface language**; the preference is remembered. Command search works in both languages at once.

> **The interface language does not decide the printed document.** The fixed labels that appear in the preview and in the PDF — WARNING / CAUTION / NOTE, Fig, Table, Procedure, Preliminary requirements, table column headings — follow **the module's own language**, that is the `languageIsoCode` in `dmIdent > language`. This is deliberate: an English data module must still say WARNING when it is printed by someone using a Turkish interface, because that module goes into an English book and ASD-STE100 fixes the word. In a bilingual publication each module prints in its own language.\
> \
> For a document with Turkish labels the module's language must be Turkish: choose `tr / TR` in the new project wizard, or produce a Turkish sibling module from an existing one with **Publication → Translate**. In a file that declares no language at all — which a valid data module will not do — the labels fall back to the interface language.

**Task panels:** new module, BOM import, BREX rules, part rows and business rule decisions open in panels docked to the right — the panel stays open while you see the result on the left, and it can be widened from its left edge.

The **sidebar** is closed and opened with the `‹` to the right of its tabs, the thin strip on the left, or **View → Sidebar**.

### Command search — `Ctrl+K`

If you do not know which tab a command is on, search for it. The tab name is printed to the left of each result, so next time you can go straight there.

- **Synonyms work:** `font`/`type size`/`paper` → Publication style; `toc`/`loep` → Front matter reports.
- **Both languages are searched**, whichever interface language is set.
- **Unavailable commands** appear dimmed and say why (“available with an IPD module open”); they do not vanish from the search.

## Documenting a new system

If you are starting from nothing rather than opening an existing data set, this is the order to follow:

1.  ### Set the project up — ✨ New Project

    The wizard asks for the project identity and writes four files into the folder you choose.

    | Field | What it is for | Example |
    |----|----|----|
    | **MIC** | Model Identification Code — the first part of every DMC | `ELD01` |
    | **Product name** | Used on the cover and in titles | Radar System |
    | **Company code** | CAGE/NCAGE — 5 characters | `TR123` |
    | **Language / country** | The file name and the `language` element | `en` / `US` |
    | **Versions** | Applicability variants | Mk1, Mk2 |

    The files created: an **ACT** (applicability cross-reference table), a **BREX** (the project's business rules), the **first descriptive module** (opened automatically) and the **publication module**.

    > The wizard **never overwrites** existing files; if a name is taken it skips that file and says so in the status bar.

2.  ### Decide the SNS

    Before you start producing modules, settle the product's breakdown into systems — `systemCode` and the codes below it come from there. The **Example SNS** folder in the S1000D package has ready-made templates.

3.  ### Produce the modules — ＋ New Module

    The project details (MIC, company, language) are inherited from the open file. Choose a type — descriptive, procedural, parts catalogue or front matter; the info code is set to match (040 / 520 / 941 / 001). Enter the technical name and the info name and adjust the DMC fields; the DMC and the file name are previewed live.

    > The technical name and info name are pre-filled by type so you can create a module straight away; both can be changed. Empty a required field and the create button is disabled, with the missing field named beside it.

    The skeleton produced is complete for its type (preliminary requirement blocks in a procedure, a first part row in an IPD) and its `brexDmRef` and ACT reference are wired up.

4.  ### Narrow the BREX to your project — ⚖ BREX rules

    - Rules are listed with **NOT ALLOWED / REQUIRED / VALUE** badges
    - **+ Ready-made rule** adds the XPaths in common use as complete rules (system code, security classification, manufacturer code…); **+ Empty rule** is for a path of your own
    - Permitted values may be **single** (`ELD01`), a **range** (`DA0~DA9`) or a **pattern** (regex)
    - On save the rules are written into the BREX module and **appear on the page at once**: a table of object path, status, description and permitted values — you can read your own rule book and hand it out as a PDF
    - From then on ✓ Validate checks every module against these rules

5.  ### Enter the IPD data

    **＋ Part row** adds one part (item, indent, part number, manufacturer, description, quantity, SMR); below the selected row if there is one.

    **⇪ Import BOM** takes the parts list from engineering as CSV: the delimiter (`,` `;` TAB) is detected, quoted fields are supported, and the columns are matched from the header names (English and Turkish). Check the preview, then import with either *append* or *replace the existing list*. Rows with no item number are numbered 010, 020, 030… automatically. This window opens by itself when you create a parts catalogue module.

    > **Service code:** for rows that carry an SMR code, the S1000D schema also requires a `service` element. The import window asks for it once and applies it to every SMR row; without it the import is blocked, because otherwise it would produce a module that fails validation.

6.  ### Define the platforms — 🎯 Applicability table

    This is the answer to one piece of content being used on several platforms: a radar may sit on a frigate, a corvette and a fast attack craft. The S1000D way is **not to copy the content** but to keep one source and mark where each piece belongs — the *Common Source* in CSDB says exactly that.

    Open the project's ACT module (`00W`); the **🎯 Applicability table** button on the **BREX** tab opens the editor. Each property is an identity and a set of values:

    | Identity   | Display name          | Values                        |
    |------------|-----------------------|-------------------------------|
    | `platform` | Installation platform | FRIGATE, CORVETTE, FASTATTACK |
    | `version`  | Version               | Mk1, Mk2                      |
    | `hull`     | Hull number           | *(empty — free text)*         |

    > **An identity cannot be changed later.** The `applic` expressions in modules refer to it, and renaming it would break those references. If you remove a property, the expressions in the modules stay as they are — they count as “undecided” when filtering.

    You state that a module belongs only to certain platforms in that module's `applic` section (Document Map › right-click › *Change…*, or XML mode). An unmarked module belongs to **every** platform.

    ### Publish — and filter by platform

    Add the module references under `pmEntry` in the PMC file — then 📄 PDF produces the cover, the contents and every module in one file.

    **One publication module, a book per platform.** With a publication open and properties defined in the project's ACT, a bar appears at the bottom of the window:

    *\[Screen diagram: Publication platform filter bar\]*

    Choose a value and the publication is **recompiled at once**: modules that do not belong to that platform stay out of the book, and platform-specific blocks inside a module are removed too. The status bar counts how many modules fell out of scope and how many blocks were filtered. With nothing chosen the publication compiles in full, as before.

    The cover of a filtered publication is **stamped with the platform it was printed for**. Two books produced from the same PMC differ only in that line; once printed, nothing else tells them apart.

    > ⚠️ **Take the “applic could not be evaluated” warning seriously.** If the tool cannot resolve an expression with certainty it **leaves the content in rather than removing it**, and counts that. Silently dropping a warning or a torque value because of an expression it did not understand is far more dangerous than leaving extra content in. There are two usual causes: a property you did not choose, or a `condition` expression tied to a service bulletin state — which the tool cannot know. Review those modules before the book goes out.

    The files never change: filtering applies only to the compiled publication, and the modules on disk stay as they are.

## Opening and saving files

1.  Choose an S1000D XML file with **📂 Open** — a `DMC-…` data module or a `PMC-…` publication module.
2.  The other modules in the same directory are listed in the sidebar; if there are unsaved changes you are asked before switching.
3.  **💾 Save** writes to the current file. The `•` on the button means there are unsaved changes.

> Graphics (ICN files) are looked for in the module's **own directory**. PNG / JPG / SVG / GIF are shown directly, CGM is converted to SVG by the built-in decoder; MP4 / MP3 open in a player.

## Visual (WYSIWYG) editing

The application opens in **Visual** mode: you see the document laid out as printed pages and work directly on it. Every change is written back to the XML in the background within half a second — DOCTYPE and entity declarations included, the source file is preserved without loss.

### Editing text

- Paragraph, title, table cell, figure caption, warning text… **click and type**.
- `Ctrl+B` bold → `emphasis`, `Ctrl+I` italic → `em02`, `Ctrl+U` underline → `em03`; subscript and superscript are preserved.
- `Enter` inside a paragraph starts a new one (a warning paragraph in a warning, a step paragraph in a step). In title fields it finishes editing.

### Adding a figure

**🖼 Figure** attaches an image to the module. In S1000D a figure is not linked by file path but through a **graphic declaration (ENTITY)** in the DOCTYPE — the application does that for you:

1.  Choose the file (PNG, JPG, GIF, SVG, CGM — CGM is decoded in the preview too)
2.  Confirm the suggested **ICN identifier** and the figure title. The identifier is derived from your company code; if the file name already starts with `ICN-` it is kept, because other modules may refer to it
3.  On Add, the file is copied into the module folder under that name, `NOTATION` and `ENTITY` declarations are added to the DOCTYPE (which is created if absent), and the `<figure>` is inserted below the selected block

### Text formatting

The **B / I / U / X₂ / X²** buttons on the toolbar (or `Ctrl+B` `Ctrl+I` `Ctrl+U`) turn the selection into S1000D's semantic elements: `emphasis`, `emphasis em02`, `emphasis em03`, `subScript`, `superScript`.

> **Why is there no font or type size?** S1000D is a **semantic** markup language: the content says “this is emphasised”, not “this is 12pt Arial”. There is no font attribute in the schema. Typography is decided by the style definition at publication time — so the same data module can print consistently in different publications and under different customers' house styles. If you want to change the appearance, the right place is the publication style, not the inside of the document.

### Untouchable items (atoms)

Cross-references (`Fig 1` `Para 1.2` `Step 2.1`), DM references, acronyms, index flags (⚑) and hotspot references behave as one piece: their insides cannot be broken, deleting one removes it whole, and they return to the XML verbatim.

### Block operations

Click a block (a blue frame means selected), then from the **Insert** tab:

| Button | What it does |
|----|----|
| ¶ Paragraph | Adds a paragraph below the selected block (inside it, if a section is selected) |
| § Section | Adds a new `levelledPara` (title + paragraph) |
| ① Step | Adds a `proceduralStep` below the selected step; the numbers shift automatically |
| • List item | Adds a new `listItem` below the selected one |
| ⚠ Warning / ⚡ Caution / ℹ Note | Adds the corresponding box |
| 🖼 Figure | Attaches a graphic file and inserts a `<figure>` |
| B / I / U / X₂ / X² | Turns the selection into semantic formatting |
| ＋ Part row / ⇪ BOM | Part entry in IPD modules |
| ⚖ BREX rules | The rule editor in BREX modules |
| 🗑 Delete | Deletes the selected block (an emptied list item goes with it) |
| ↩ Undo | Undoes the last structural change or text session |

### Tag view

The **⟨⟩ Tags** button draws `⟨para⟩` … `⟨/para⟩` markers around every block — for editing with the document structure in sight. These markers never enter the PDF.

## XML mode

The **Visual \| XML** switch at the top moves to the source editor: syntax highlighting, code folding, line numbers. Changes reach the preview on the right within about 0.4s; broken XML is reported at once with a red error strip.

## Validation — ✓ Validate

One button runs two checks at once:

1.  **XSD** The document is validated against the schema named in `xsi:noNamespaceSchemaLocation`, using the bundled S1000D schema package — it ships with the application, no internet needed.
2.  **BREX** The BREX module pointed to by the document's `brexDmRef` is loaded from the directory (or the bundled S1000D default BREX if it is missing), and every `structureObjectRule` is evaluated.

- Click an **XSD** finding → the matching **line** is selected in XML mode.
- Click a **BREX** finding → the matching **element** is selected and scrolled to in the preview.

## Module properties — 📋 Properties

The data module's identity and status information, edited without going to XML mode: technical name and info name, **security classification** (01 Unclassified … 05 Top Secret), issue type, issue number / inWork / date, responsible company and the applicability text.

> Fields the module does not carry are greyed out with “Not in this module”. The element order of this section is strict in S1000D; guessing where a missing element goes would make the file invalid. You can print the security classification on every page by putting `{security}` in the publication style.

## Publication style — 🎨 Style

Appearance is defined not in the data modules but in a **publication style belonging to the project**. S1000D is a semantic language: the content says “this is emphasised”, not “12pt Arial”. The style drives both the preview and the PDF, and is saved into the project as `s1000d-style.json`.

| Section | Settings |
|----|----|
| Preset | S1000D default, Compact, US Letter, Security stamped |
| Page | Paper (A4 / Letter / A5), orientation, all four margins (mm) |
| Header and footer | Three fields, left / centre / right; tokens or plain text |
| Typography | Body and heading typeface, size, line spacing, heading scale, rules |

The tokens: `{dmc}` `{title}` `{techName}` `{infoName}` `{issue}` `{date}` `{security}` `{enterprise}` `{page}` `{pages}`. Clicking a token button copies it to the clipboard. Changes reach the preview at once; **Save to project** makes them permanent.

> Fields such as the security classification are read from the data module — put `{security}` in the header or footer and each page prints that module's own classification.

## Bulk import — 📊 (Home tab)

Rather than producing modules one at a time you can create them **in bulk from a CSV list**, or revise an existing project in bulk. The two ready-made workbooks in the `toplu-islem/` folder are for this; fill one in and save it as CSV.

| File | What it does |
|----|----|
| `01-yeni-proje-toplu` | Each row is a data module to create — template, DMC parts, titles, security, graphic (ICN) |
| `02-guncelleme-toplu` | Each row is an existing module; the Action column (update / new issue / rename / delete) decides what happens |

The panel shows the **row-by-row plan before anything is applied**: how many modules will be created or updated, which rows will be skipped and why. Bad rows are skipped and the rest are applied.

> On creation, existing files are **never overwritten**. On update the files are edited at text level: only the changed lines change, and the DOCTYPE and graphic declarations are preserved — the diff in version control shows the revision and nothing else.

> Working from CSV, you only need to fill in the **Action** column; the issue type (`changed`/`revised`/`deleted`) is derived from it.

## The S-Series bridge — 🔗

Within the ASD/AIA S-Series family, S1000D covers only **publication**; parts provisioning (S2000M), preventive maintenance analysis (S4000P) and logistics support analysis (S3000L) are separate specifications. These four commands carry that other specifications' data **directly** into S1000D data modules — without retyping it.

> All four are **context-dependent**: they stay greyed out until the right kind of module is open. What each one needs is listed below — faster than hunting for it in the ribbon.

### S2000M → IPD

With an IPD module open: **Home → ⇪ Import BOM**. The file's **content** is inspected, not its extension: if it is an S2000M provisioning message (`locipd`), column mapping is skipped entirely — part number, manufacturer code, description, the quantity per next-higher assembly and the **NSN** are read directly. The same command also accepts a plain CSV, in which case the column-mapping step appears.

> S2000M keeps the NSN split across two fields (supply class + item identification number); S1000D splits it into three. The conversion is automatic, and the manual-entry form carries the same NSN column.

### S4000P → Scheduled maintenance

This one needs a **scheduled-maintenance (`schedul`) module** first — create an empty one with **＋ New Module → Maintenance plan / time limits**. With that module open, **Home → ⏱ Import S4000P** appears: the threshold value, its unit and the limit's description are read directly from the S4000P analysis file you pick (`pmaDataset`).

> S1000D's `thresholdUnitOfMeasure` list is a **closed** set of codes; S4000P uses its own vocabulary, so a unit conversion happens and is **shown** in the preview. A row marked "≈" means both definitions describe the same thing but S1000D's equivalent is narrower or wider — review it before importing. Units with no S1000D equivalent can be assigned a project code (`th51`-`th99`) in the [mapping profile](#sseries-profile); left unassigned, that threshold is skipped, and the import states how many before you commit.

### S3000L → Procedure skeleton

With project parameters defined (a project open): **Module → ⚙ Module from S3000L**. Unlike the other three, this one does not edit the open document — it **writes new files**: one empty-bodied procedural (`proced`) module per usable task in the LSA data set (`lsaDataset`) you pick. A task's information code (`taskInformationCode`) maps directly to the S1000D information code; if the LSA declares a target data module (`resDM`) its DMC is used, and if it does not, the DMC comes from the defaults in the [mapping profile](#sseries-profile) — the preview shows, row by row, which source each one came from.

> ⚠️ **Step text is never generated.** LSA states how long a task takes, not how it is performed; the body arrives with a "Write the steps here" placeholder for the author to fill in. If two tasks land on the same DMC (different parts, the same information code, say), the second is **not produced** — rather than overwrite the first, the conflicting tasks are reported.

### Mapping profile

**Module → 🔗 Mapping Profile** (with any module open). Both decisions edited here belong to the **project**, not the specification: a project code for S4000P units with no S1000D equivalent, and DMC defaults (system, sub-system, assembly...) for when S3000L does not declare a target module. It is written to the project folder as `s1000d-mapping.json` and read automatically on every later import.

> **Only th51-th99 are accepted.** th01-th39 are S1000D's own definitions (Table 81); mapping an S4000P unit there would assert a meaning nobody has verified. A unit that already has a standard equivalent cannot be **overridden** by the profile — the profile does not replace the specification, it only fills the gap the specification deliberately left.

### The other direction: the S3000L catalogue

With a project folder open: **Publication → ↗ S3000L Catalogue** — no dialog, one click. It scans every data module in the folder (files prefixed `DMC-...`) and produces the document records the LSA's `resDM` declarations point to, writing them to `S3000L-belge-katalogu.xml` in the same folder. The four flows above all ran one way, from S-Series into S1000D; this one reports back what the publication side actually produced. The generated message validates against the real S3000L 3.0 schema.

> All three import panels carry a collapsible **diagnostic note** (closed by default): it counts which containers in the file you picked are populated, how many were read, and why the rest were left out of scope. When fewer rows come out than you expected, this is the first place to look — it tells you whether to blame the file or the tool.

## Translation — 🌐 Translate (Publication tab)

S1000D does not translate a module in place: a translation is a **parallel-language sibling module with the same DMC** (for example `…_EN-US.XML` → `…_TR-TR.XML`). Open the module you want translated and choose **🌐 Translate** on the Publication tab.

| Step | What it does |
|----|----|
| Target language | Chosen from tr-TR, en-US, en-GB |
| Segment list | The module's text is listed piece by piece, with the translation typed under each line. A context badge (`para`, `title`…) shows where the text came from |
| Translation memory | If the same text occurs in several places, translating it once fills the rest (↻ from memory). “Show only untranslated” filters down to what is left |
| Create the translation | The parallel module is written and opened; codes, cross-references and inline formatting are preserved exactly, and only the words change |

> What you type is added to the project's **translation memory** (`s1000d-tm.json`), which then fills repeated sentences in later modules by itself. A partly translated module can also be produced — untranslated segments stay in the source language.

> ⚠️ **There is no cloud translation.** The tool is offline and no text is ever sent out — for defence and naval documents, translation happens with an embedded model's suggestion, human approval and a local translation memory.

> **Embedded offline translation:** the panel has a 🤖 *Auto-translate blanks* button. The embedded model fills empty segments with a **suggestion** (marked 🤖); the suggestions are raw output, so **review and correct every line**, and use ↶ *Undo* if needed. The model is inside the application — no text leaves, and no network is required.

> ⚠️ **If translation does not start:** the button no longer waits in silence. If the engine cannot start, the *reason* appears in a red line in the panel (for example the full path of a missing file). That text can be selected and copied — send it when reporting a problem, it is the only diagnosis there is. Until 0.28.0 nothing was shown in this situation and the button stayed on “Translating…”.

> **The installed language pair:** English → Turkish. The target language list is limited to Turkish and English as well — these publications are produced in Turkish and English, and keeping an unused language in the list brought nothing but the risk of creating a sibling module in the wrong one. Choose a target with no model and a line appears in place of the button saying so; the rest of the panel — the segment list, the translation memory, partial translation, parallel module output — keeps working.

> **Where the models come from:** Mozilla's Firefox Translations “tiny” models, taken from the Remote Settings collection Firefox itself downloads. `scripts/fetch-mt-models.mjs` verifies every file against the SHA-256 published in that registry; on a mismatch the build stops. Downloading happens only at build time — the installed application never reaches the network, and the model bytes are read from disk. If another language is needed later (German, French, or the reverse direction Turkish → English), uncomment the line in that script and run `node scripts/fetch-mt-models.mjs`; the panel asks the application which pairs are installed, and nothing else in the code changes.

## Producing a PDF

### A single module

**📄 PDF** exports the current module as A4: the DMC in the header, the title and page number in the footer. The preview and the output share one style; graphics, CGM included, are embedded as vectors.

### A publication (Publication Module)

Open a `PMC-…` file and the preview compiles the whole publication: the cover, a Contents page and every data module in `pmEntry` order. Modules are found in the directory by their DMC prefix, the highest issue is taken when there are several, and missing ones are marked in red as “not found”.

> ⚠️ The Contents page carries no page numbers (a limit of the Chromium print engine); its entries are clickable links inside the PDF. For a formal table of contents with page numbers, use the front matter reports.

### Page view

With **View › Page › 📄 Page View** on, the preview is laid out not as one continuous column but as **real A4 sheets**: margins, headers, footers and page numbers are visible on screen.

This is more than a view: **the PDF is those very sheets.** Export hands the same paginated document to the printer with zero margin and one sheet per page. If you see 12 pages on screen the PDF has 12, and the lines end in the same places.

| Command (View tab) | What it does                                  |
|--------------------|-----------------------------------------------|
| 📄 Page View       | Sheet layout ↔ the classic continuous preview |
| ↻ Re-flow Pages    | Split the pages again by hand                 |
| ＋ / － / ⤢        | Zoom in · zoom out · fit page width           |

**The zoom bar:** the same four commands also sit in the status bar along the bottom edge of the window — *📄 Page View*, *－*, the current percentage, *＋* and *⤢ Fit page width*. They are there so you need not go to the ribbon; both places run the same commands, not separate copies. The percentage is the one *applied*: with fit-to-width chosen it shows the factor the window worked out, and it changes as you zoom. With page view off nothing is scaled, so it reads *—* and the buttons are disabled.

If typing pushes text past the bottom of a page, the sheets re-flow when you pause and the caret stays where it was. The status bar shows the total page count.

> A block that is longer than a page on its own (a single table row holding hundreds of codes, say) cannot be split; it is **scaled down** to fit that page and reported in the status bar as “*n* blocks scaled down”. The scaling is the same in the PDF — content is never silently clipped.

## The S1000D issue — Issue 6 and Issue 7

A CSDB is at **one issue**: every module in it declares the same schema package, and its BREX and business rule decisions belong to that issue. S1000D asks this as a decision point of its own (BRDP-S1-00003). The issue is therefore a *project* setting, not a per-file one.

You choose the issue in the **New Project** wizard; the default is **Issue 7**. When you open an existing data set the application reads the issue from the files themselves, so your older Issue 6 projects keep working as they are, and new modules produced inside them declare Issue 6 too.

| What | How it behaves |
|----|----|
| Validation | The issue in the document's own schema declaration is read, and it is checked against that issue's package |
| Default BREX | Chosen by issue (Issue 6: 255 rules, Issue 7: 263) |
| Generation | Modules, the ACT, the BREX and the publication module declare the project's issue |
| Older samples | Files declaring Issue 5.0 are met with the Issue 6 package rather than rejected |

> Issue 7 is a superset of Issue 6: 22 elements were added, none removed, and no value list this tool uses changed. An Issue 6 module therefore also passes the Issue 7 package — but that is not a reason to mix the two: writing an Issue 7 module into an Issue 6 CSDB leaves the data set declaring two issues at once.

## Project profile — 🎛

S1000D covers air, land and sea vehicles, interactive publications, training content and service bulletins together. If your project does not use all of them, what is unused can be hidden from the interface.

| Profile | Decision points | Groups hidden |
|----|----|----|
| Generic (everything) | 432 | — |
| Land/sea, no extra capabilities | **396** | 24 BDAR · 29 air vehicle · 33 service bulletin · 34 training · 38 IETP |

Out-of-scope module templates also disappear from the Module tab (the Process module, for instance, while IETP is off). **Nothing is deleted** — it all returns from **Home → 🎛 Project Profile**. The profile is written into the project folder as `s1000d-profile.json`; in older projects that have no profile, everything stays visible.

## Business rules — ⚖

S1000D business rules live in two modules, and the two are halves of one thing:

| Module | Code | Contents |
|----|----|----|
| Business rules document | `024` | The decision itself and its reasoning — read by people |
| BREX | `022` | The machine-checkable part of those decisions — applied by ✓ Validate |

### Decision points (BRDP)

The specification asks every project the same **432 decision points**. The application carries that index embedded; with a business rules module open, the **Business Rules** tab appears and opens the **Business Rule Decisions** panel.

- Search across title, definition, BRDP code and specification section; filter by group and category
- An **All / Pending / Decided** filter, with “N / 432 decided” at the top
- Click a decision for its definition, its categories and **the values S1000D permits**; choose one and write your reasoning
- A saved decision is written into the 024 module, with the group headings falling in specification order

> **Optional BREX link:** tick “also add this as a BREX rule” and the rule is written into the project BREX and tied to the decision with `brDecisionRef`. It is off by default — most decisions are managerial, and whether to add them to the document is yours to judge.

## Front matter reports — 📑

With a `PMC-…` file open, this button derives the specification's front matter reports from the publication itself. These reports are not written by hand: their contents are a restatement of information already in the publication module and the data modules.

| Report | Info code | Contents |
|----|----|----|
| Table of contents (TOC) | `009` | The publication module's title hierarchy, DMC, issue date, page count, applicability |
| List of effective pages (LOEP) | `00R` | For every printed page: module code, page number, C/N mark, date |
| List of effective data modules (LOEDM) | `00S` | Every module in this issue, with C/N marks |
| Highlights | `00U` | The changed modules and their `reasonForUpdate` texts |

Page counts are **measured from the real layout**: the publication is laid out in the background at print page size and the number of pages each module occupies is read off. If the paper, the margins or the type size change, produce the reports again.

Change marks come from each module's own issue date: modules dated the same as the publication count as changed in this issue (new, N, when `issueNumber` is 001). If the previous issue's LOEDM file is in the folder, Highlights also produces the “remove/insert” update instruction (fm04).

> The change record (CR) and the technical standard record (TSR) cannot be derived — they are forms filled in by hand, and they come as templates under **＋ New Module → Front matter**.

## Supported content

| Schema / type | Coverage |
|----|----|
| `descript.xsd` | Sections (numbered automatically), paragraphs, lists, CALS tables, figures, warning/caution/note |
| `proced.xsd` | Preliminary requirements (conditions, personnel, support equipment, supplies, spares, safety), numbered and alternative steps, requirements after job completion |
| `frontmatter.xsd` | Title page, Table of contents, LOEP / LOEDM / Highlights |
| `pm.xsd` | Publication assembly: cover + contents + modules |
| `ipd.xsd` | Parts catalogue: exploded view plus an Item / Part number / Mfr / Description / Qty / SMR table; indent levels, variant rows; data entry by form and by CSV |
| `brdoc.xsd` | Business rules document: decision points, categories, the decisions taken |
| `comrep.xsd` | Common information repositories — tools, supplies, zones and access points (zoning, Ch. 3.4) |
| `brex.xsd` | Business rules: the rule table is shown on the page, edited in the editor and applied by ✓ Validate |
| `fault / schedul / crew / checklist / comrep / wrngdata / process / sb` | Chapter 3 information sets: fault reports, maintenance planning limits, crew information, checklists, common information repositories, wiring tables, process and service bulletin — the first seven can be produced from a template in the wizard |
| Graphics | PNG, JPG, SVG, GIF (animated), CGM (built-in decoder), MP4 / MP3 |

Unrecognised elements are processed with their content preserved; partially supported modules can still be previewed and edited.

> The `public/samples/` folder holds 60+ examples from the Bike Data Set: descriptive and procedural modules, front matter, BREX, IPD, a publication module and its graphics. Start experimenting there.

## Common situations

| Symptom | What to do |
|----|----|
| The ＋ New Module button is disabled | Open a file from the project first — the wizard inherits the project details from the open file. |
| Columns are empty in a BOM import | Pick Part number and Description by hand in the column mapping menus; those two are required. |
| An `[ICN-…]` box where a graphic should be | The file is not in the module's directory, or its format is unsupported — put the ICN file in the same folder as the XML. |
| “Publication: N data modules, M missing” | Some DM files the PM points to are not in the directory; the red lines in Contents show which. |
| Windows SmartScreen warning | The package is unsigned: click **More info** → **Run anyway**. |
| Something was deleted or broken in Visual mode | **↩ Undo** reverses the last operations; `Ctrl+Z` works in XML mode too. Nothing changes on disk until you save. |
| Auto-translate does nothing | The panel shows the engine's own error in a red line. Copy that text — it names the cause. |

## Release history

What changed in each version, newest first. The version you installed is named in the colophon at the foot of this page.

- **0.45.4** — **A splash screen at start-up.** The desktop application now opens with a splash screen showing the version, the author (Rahmi Gökhan VURAL) and the contact address (rgokhanvural@outlook.com). It stays for at least 5 seconds before the main window appears; if loading hangs it closes after 20 seconds at most.
  - The splash used to exist in the evaluation build only. There the evaluation details (issued by, issued to, valid until) stay in place, with the author and contact lines added below; the minimum is still 10 seconds.
  - The splash **can no longer be dismissed with a click**, and the "click to skip" hint is gone.
- **0.45.3** — **Line endings are now preserved.** A file written with CRLF line endings came back with an LF body after being edited and saved. The cause sits at both ends of the chain: XML parsing normalises CRLF to LF before the text ever reaches the DOM, and `XMLSerializer` emits LF. The prolog, which is sliced out as text, kept its CRLF — so the file ended up with **mixed line endings** and the module showed as *every line changed* in a diff. For an author working on Windows that made version history and review unreadable. The source's own convention (by majority) is now restored; LF files are unaffected, not a byte differs.
  - **The new test suite found this.** `npm test` now chains eleven checks alongside the typecheck and runs **306 assertions**. New ones cover round-trip and format preservation, renderer security (XXE, entity expansion, deep nesting, oversized values), validator security and path traversal, the XSD validation contract, and schema-declaration reading.
  - **Renderer tests run in real Chromium** (`scripts/lib/browser-run.mjs`, using Electron's own engine — no new dependency), because the product parses XML with the browser's `DOMParser`; testing that with a different parser in Node would measure something else.
  - Fidelity measured and pinned: comments, CDATA, processing instructions, namespaces, mixed content, **attribute order**, indentation, `xml:space`, Turkish characters and the DOCTYPE internal subset all survive byte for byte. The `<b></b>` → `<b/>` and single- to double-quote normalisations remain (identical meaning) and are recorded in the tests.
- **0.45.2** — **A file whose schema declaration contained a line break was read as having no declaration at all.** A data module names its schema in the root element's `xsi:noNamespaceSchemaLocation` attribute. When the validator pulled the schema file name out of that address, it required the closing quote to sit **immediately** after it (`…descript.xsd"`). XML, however, permits whitespace — a newline included — inside an attribute value and normalises it to a space. A published ASD sample wraps the line right before the quote, so a file whose declaration was plainly there was reported as *"no xsi:noNamespaceSchemaLocation found"* — while the issue was being read successfully from that very same attribute. The asymmetry came from the two patterns having been written separately.
  - **The patterns now live in one place** (`shared/schemaDecl.cjs`), pulled in by both the desktop and the server host. The copy was the root of the defect: the same contract expressed twice, one of them gone stale. `scripts/check-schema-decl.mjs` checks that all three copies (two hosts plus the browser) say the same thing.
  - **Three files in the in-app sample corpus were repaired.** One would not even parse: a Turkish translation carried the control character `0x03` in 38 places where spaces belong. Two declared `frontmatter.xsd` while carrying a descriptive body; their declarations were brought in line with the body, the content untouched. Every deviation is recorded in `public/samples/OKUBENI-sapmalar.md`.
  - All **215 of 215** files across the three sample corpora now pass the product's own validator (211 before).
- **0.45.1** — **The new-module wizard stayed on screen after switching to another ribbon tab.** Reported by the user: after adding a module, picking a different tab left the panel on the right hanging open. Cause: the panel is deliberately kept open across a file load so several modules can be created in a row (bulk import works the same way), but tab-switching was never covered by that rule — nothing read it as "done with this now." Switching away from the panel's own tab (Module / Home) now closes it; staying on the same tab is unaffected. **Also: the editor now asks the schema before it writes.** The delivery-package check was failing on all four issues, and one sample module was the cause; its two faults were the traces of two faults in the editor.
  - **Block insertion follows the content model.** In an IPD module, pressing "Paragraph" with nothing selected wrote a paragraph into `<content>` — which accepts no paragraph in any issue. It worked by accident in a descriptive module only because the search found `content > description` first. The editor now asks the schema before placing anything: the block goes into the nearest container that accepts it, and when nothing does, it is not inserted and the editor says so. **Order is honoured too** — `illustratedPartsCatalog` does accept a figure, but its model is a sequence and the figure must precede `catalogSeqNumber`; appending produced an invalid file. The table is generated from the bundled XSDs (`scripts/build-content-model.mjs`) and is keyed by issue, because the issues are NOT nested supersets: `commonInfoDescrPara` accepts a warning in 4.1 and 5.0 but not in 6 and 7. When a document declares no issue, the intersection of all four is used.
  - **Data module code fields are validated against the schema.** All eleven wizard fields were free text and the value was never checked; `itemLocationCode="E"` came from that (the schema allows only A, B, C, D, T). The hole was not in one field: ten are pattern-constrained, so a lowercase letter or an extra character failed just as silently. Each field is now checked against its own constraint, a non-conforming code no longer creates a module, and the panel names the field and the reason. The constraints are identical across all four packages, so there is a single table — and the generator halts the build if they ever diverge.
  - **The broken sample module was repaired.** The "Tools" IPD in the ship data set carried both the invalid location code and a paragraph that had landed in `<content>`. The location code became `C` (items on the bench, Chap 4.3.8), which fits the module's own subject — `D` would have collided with its sibling's DMC — and the file name changed with the code.
  - `scripts/check-content-model.mjs` covers all three: the table is compared against 426 real pairs in the sample set, the output of each insertion scenario is validated with xmllint, and the field rules are applied to 57 real DM codes.
- **0.45.0** — **Mapping profile, reverse direction and a diagnostic report.** The parts of S-Series integration the specifications leave to the project.
  - **Mapping profile** (Ribbon → Module → *Mapping Profile*). Two decisions now live with the project: a project code (`th51`–`th99`) for S4000P units S1000D has no equivalent for, and the DMC fields to use when an LSA does not declare the resulting data module. Stored as `s1000d-mapping.json` beside the modules. **Only th51–th99 is accepted:** th01–th39 are S1000D's own definitions (Table 81), and mapping an S4000P unit onto one would claim a meaning we have not verified. A unit with a standard equivalent **cannot be overridden** — the profile does not replace the specification, it fills what the specification leaves open.
  - **Reverse direction: S3000L document catalogue** (Ribbon → Publication → *S3000L Catalogue*). Builds, from the data modules in the folder, the document records an LSA's `resDM` declarations point at. The four earlier flows were one-way; this reports back to the analysis side what publication produced. The generated message validates against the real S3000L 3.0 schema.
  - **Diagnostic report** — *"What is in this file?"* in every import panel. It counts the recognised branches in the source, how many objects each holds, what we read and **what we do not**. When an import yields three rows from a three-thousand-line file, what was skipped is now visible, with the reason.
  - `scripts/check-s-series-extras.mjs` exercises all three (29 checks).
- **0.44.0** — **Data module skeletons from S3000L tasks.** Each task in a logistics support analysis becomes an empty-bodied procedural (`proced`) module with its DMC, technical name, info name and a task note in the preliminary requirements. Ribbon → Module → **Module from S3000L**.
  - **No mapping table was needed for the information code.** S3000L's `taskInformationCode` values *are* S1000D information codes: 240 Lubrication, 280 Inspection, 520 Remove, 920 Change — verified against the specification text. (The opposite of the S4000P units, where two separate vocabularies made a conversion table unavoidable.)
  - **The LSA already declares the target module.** `resDM` on the task revision points at the data module the task produces; that DMC is read and used **as given** — we do not invent it. Where it is absent the DMC is derived from the project parameters, and the preview marks every row accordingly: a DMC the customer supplied and one we assumed do not carry the same weight.
  - **Step text is not generated.** An LSA states how long a task takes, not how it is performed. An invented step is work the author must delete — and, if missed, something that ships. The first step instead carries the task id, its duration and the information code.
  - A task without an information code produces no module (the code is a required part of the DMC); a second task landing on the same DMC is skipped and reported. Existing files are never overwritten: a module generated earlier and since filled in must survive a second import.
  - Modules are not hand-built; the application's own `generateModule` is called, so they carry the same identity, status and BREX reference as a module created through the wizard.
  - `scripts/check-s3000l.mjs` exercises four stages (28 checks): the sample LSA dataset against the real S3000L 3.0 schema, the codes against the S1000D pattern, the parsed tasks against expected values, and the generated modules against **proced.xsd in all four generations**.
- **0.43.0** — **Time limits from S4000P preventive maintenance analysis.** Maintenance intervals can now be read from a PMA dataset into a scheduled maintenance (`schedul`) module: threshold value, unit and the limit's description come across directly. Ribbon → IPD → **Import S4000P**.
  - **Units are converted, and the conversion is shown.** S1000D's `thresholdUnitOfMeasure` is a **closed** code list (`th01`…`th39`) and is required; S4000P uses its own vocabulary. 13 of 52 S4000P units are mapped. The mapping is not guessed: S4000P tags every unit code with its SX001G term, and the S1000D meanings come from Chap 3.9.6.1, Table 81.
  - **A threshold in an unmapped unit is not written.** An invented code is a silently wrong maintenance interval. How many thresholds were dropped, and for which unit, is stated before the import. S1000D reserves `th51`-`th99` for projects; such units belong there (no UI for that yet).
  - **"≈" marks** conversions where the two definitions agree but S1000D's is narrower or wider (a plain "hour" becoming "elapsed hours", for instance). Not an error — a translation to review.
  - Time limits are not added when the module's `maintPlanning` already holds an inspection, task or allocation branch: S1000D allows one kind there, and mixing them makes the module invalid.
  - `scripts/check-s4000p.mjs` exercises four stages (30 checks): the sample PMA dataset against the real S4000P 3.0 schema, the unit table against Table 81, the parsed rows against expected values, and the produced module against **schedul.xsd in all four generations**.
- **0.42.0** — **IPD from S2000M provisioning data.** A parts list can now be read from an **S2000M message** as well as from CSV: part number, manufacturer code, description, quantity in the next higher assembly and the **NSN** come across directly, with no column mapping. The file is recognised by its content, not its extension.
  - **The NSN now reaches the catalogue.** S2000M holds it in two fields (supply class + item identification number) and S1000D splits it into three; the conversion is automatic. Manual entry gained an NSN column too.
  - **FIX — rows without a manufacturer code produced an invalid catalogue.** `manufacturerCodeValue` on `partRef` is **required** in all four generations, but the code wrote it only when present. Every CSV import that left the manufacturer column empty produced a module that opened correctly and failed the recipient's validator. Such rows are now **not written**, their count and cause are reported **before** the import, and the result message states the number of rows actually written rather than the number offered.
  - Only the `locipd` payload is read; `pnoipd`/`splinf`/`seripd` carry no figure item number or quantity and cannot form an IPD row — such a file is rejected with the reason stated instead of returning silently empty.
  - `scripts/check-s2000m.mjs` exercises three stages: the sample message against the real S2000M 8.0 schema, the parsed rows against expected values, and the produced catalogue against **ipd.xsd in all four generations**. The S-Series schemas are XSD 1.1 (`xsd:assert`) and xmllint is a 1.0 processor, so stage 1 validates against a copy with the assertions stripped — structure and types are checked, co-occurrence rules are not.
- **0.41.0** — **SX001G terminology layer.** The glossary shared by the six S-Series specifications now ships inside the editor: **3,696 terms**, half of them with their official definition. The translation panel recognises a standard term inside a segment and shows its definition on hover.
  - **You decide the equivalents.** SX001G has no official Turkish edition and we do not invent one. The project glossary (`s1000d-terms.json`) starts empty; once you approve an equivalent for a term, every segment that does not use it is flagged. Terms with no approved equivalent appear as a **to-do list**, not a warning — in a different colour, because the author has done nothing wrong.
  - **The term list comes from the schemas, not the PDF.** Each of the six XSD packages tags every field with a machine-readable source; identifiers such as `taskInformationCode` are expanded into the words an author actually writes ("task information code"). Regenerate with `node scripts/extract-sx001g.mjs && node scripts/build-sx001g-module.mjs`.
  - **Single-word terms are not flagged automatically.** The glossary also holds ordinary words such as `assembly` and `ampere`; warning on every occurrence would make the panel unusable and train the author to ignore the real warnings too. They remain searchable.
  - The glossary loads only when the translation panel opens (758 KB, separate chunk), so a session that never uses it pays nothing. `scripts/check-glossary.mjs` exercises both the data and the matcher with 22 checks.
  - **Only author rights may change the project glossary** (web edition). Unlike the translation memory, a wrong equivalent propagates into every later module. Reviewers can still translate.
- **0.40.0** — **Evaluation builds.** The same source now produces a copy that carries, inside itself, who it was given to and how long it is valid for. Run `node scripts/make-trial.mjs --for "X Ministry" --until 2026-10-12`, build, then `--clear`. **No file means a normal build** — all a normal package gains is an attempt to read a file that is not there.
  - **A splash screen** naming the recipient and the expiry date. It stays for at least 10 seconds but **a click dismisses it**: making an evaluator who opens the application thirty times a day wait has nothing to do with deterrence.
  - **A persistent badge and window title.** The splash is momentary; this is the part that does the work. The badge turns to the warning colour in the last week — a tool that locks ends the conversation, a tool that warns gets the vendor a phone call.
  - **A one-line notice at the foot of each PDF page.** A large diagonal stamp was deliberately not used: this product's strongest claim is *"the page on screen is the page that prints"*, and a stamp would hide exactly that from the person judging it. The notice is **in the preview as well** — something present in the PDF but missing on screen would undo the claim — and because it sits outside `.sheet-body` it **does not move a single page break**: an evaluation copy paginates exactly like the copy they would buy.
  - **A note file in the delivery archive** (`DEGERLENDIRME-KOPYASI.TXT`) and `-DENEME` in the archive name. **No S1000D file is touched**: the DML, DDN and IMFs still validate, and the data modules are byte for byte identical. Deleting the note is easy; the point is not to make it hard but to make it deliberate.
  - **An expired build does not open.** It closes before the window is created, with a message saying what happened and that the projects are untouched.
  - **Nothing is written into the XML.** Lossless round-trip is what this product is sold on; stamping the files would break schema validity and that claim together.
  - None of this stops a determined person — the package is unsigned and the code is JavaScript inside an asar. The aim is not to make unauthorised use impossible but to **stop it being accidental, and make it provable.**
  - Side finding, worth recording: the preload runs **sandboxed**, and `fs` does not exist there. Reading the licence file in the preload leaves `window.api` undefined, and the application then opens the **web shell** instead of the desktop one — with no error in the console. The details now travel from the main process through `additionalArguments`.
- **0.39.1** — **The DML/DDN/IMF the delivery package writes now validate.** The desktop *File → Delivery package…* route was exercised end to end for the first time (menu event → renderer → main process → builder → save). The route worked, but the identification files it produced **failed schema validation** — nobody had measured that: the archive opens, the files are all there, and only the recipient's validator would reject it.
  - **Mandatory elements that were missing in all four generations.** **`brexDmRef`** in `dmlStatus` and `ddnStatus` (pointing at the project's own BREX when it has one, at the S1000D default otherwise), **`authorization`** in `ddnStatus`, and **`responsiblePartnerCompany` + `originator` + `qualityAssurance`** in the IMF. The DDN wrote its security classification as an attribute of `ddnStatus`; the schema wants a **`security` child element**.
  - **The DDN's sender and receiver addresses (`dispatchTo` / `dispatchFrom`) are now always written** — both are mandatory, and they were omitted whenever the details were unknown. They come from `s1000d-delivery.json`; rather than inventing an address for a mandatory field the file does not fill, a visible **`TBD`** goes in **and the status bar names the missing fields**. On the Web Edition the same warning lands in the audit trail and in an `X-Delivery-Warnings` response header.
  - The settings file grew: street, postcode, city and country for both parties, plus the authority that authorizes the delivery (`authorization`).
  - Measured: the sample naval publication (63 files, 8 graphics) was converted to each of the four generations and packaged; the resulting **DDN, DML and all eight IMFs** validate against the bundled XSDs. The package produced on the desktop was checked separately: 73 files, DDN first in the archive, names flat and upper-case.
- **0.39.0** — **Signing out really ends the session; the Turkish MoD national layer; the delivery package on the desktop.** Everything left on the agenda.
  - **An author who signed out stayed "online" on everyone else's screen.** Reported by the user. Measured: `/api/logout` released their locks but **never dropped their presence**, so they lingered on the project board until the TTL expired — **90 seconds**. Because sign-out works on your own screen, the fault is only visible from a second user's. Presence is now released with the locks and the board updates at once; a second tab left open re-announces itself on its next heartbeat (60 s).
  - **The Turkish MoD national business-rule layer.** One choice in the project profile: *None* or *MoD (Turkey)*. With it on, the security classification list shows the **13 national values** instead of the specification's generic five (NATO Restricted, Cosmic Top Secret, EU Confidential…), and the data module code is checked against **Appendix 3's field rules** (`modelIdentCode` 2–14 characters, and so on) — the limit the schema does not catch but the requirement document asks for. The values live in the 51–99 range S1000D **reserves for national use**, so a module produced with the layer on stays conformant. The layer is generated from the published workbook with `python3 scripts/build-msb-layer.py`: 7 attributes, 71 values, 13 code rules.
  - **The delivery package is on the desktop too** — *File → Delivery package…* and a ribbon button. The builder was not written twice: the ZIP writer and the package builder moved to shared modules (`shared/zip.cjs`, `shared/deliveryPackage.cjs`) and both hosts run **the same code**. That is deliberate: the package's issue declaration went stale in 0.35.1 for exactly the "same thing in two places" reason.
  - **Sender and receiver identity is a project setting now.** The modules know the sender's code but not the receiver — that belongs to the delivery itself. It sits in the folder with the other project settings: `s1000d-delivery.json`. Without the file the behaviour is unchanged, and the file never travels in the package.
  - **The bulk and translation panels stay open after an operation.** The module wizard already did; these three closed, forcing an author working through several batches to reopen them each time. The outcome is reported in the status bar either way.
  - **A "who did what" indicator.** The live stream already carried the event but nothing rendered it. The project board now shows the last action beside the online list: who, what they did, and to which file.
- **0.38.0** — **The ribbon icons are no longer emoji: a designed 71-piece single-colour set has landed.** 0.37.0 grouped the ribbon by function but left the icons as emoji — and the measurement showed **seven emoji shared across seventeen slots** (📄 stood for four different jobs). Every command now has its own drawing; the icon distinguishes.
  - **One colour, `currentColor`.** The icon takes the button's colour, so a disabled command's icon fades with it and an active one is highlighted. No colour is hard-coded in the files.
  - **Emoji rendered differently on every platform**, and the product runs on Windows, macOS and in a browser. Colour emoji looked like a toy on a defence customer's screen.
  - **The package did not grow.** 97% of the delivered SVGs was C2PA provenance metadata (measured: 552 KB of metadata, 5.9 KB of drawing). The embedding script strips it; `src/icons.ts`, which is what ships, is **8 KB**. The source files stay untouched under `docs/design/`.
  - **Embedded as one module**, not 71 requests — the product runs offline on air-gapped networks.
  - **The emoji fallback stays for one more release:** a new command without an icon prints its old placeholder rather than an empty box. Verified: across all six visible tabs every one of the 69 icons rendered as SVG, with no emoji left. The stroke width was left at the designer's value (2) — that is 1.33 px at 16 px and 2.25 was tried, but no distinguishable difference could be measured on screen.
- **0.37.0** — **The ribbon is grouped by function now: 20 groups instead of 33.** The same 71 commands, none removed; what changed is how they cluster. Over time the ribbon had come to follow S1000D's schema families rather than the work the author is trying to do.
  - **The Module tab went from 12 groups to 5.** Twelve headings, eight of them carrying a single template (Fault, Planning, Crew, Checklist, Wiring, Process…). Now **Description and procedure** (6), **Maintenance and fault** (4), **Parts and repositories** (5), **Rules and crew** (2), **Front matter** (7).
  - **All four groups on the Publication tab held one button each** — four headings for four buttons. Now **Prepare** (style, front matter, translation) and **Output** (PDF).
  - **The validation tab is one group.** Validation and business rules are two sides of the same job; the tab was also called "BREX", which described half of what was in it — it is **Validation** now.
  - **View went from 6 groups to 4.** The single-command "Show" group joined Mode, and help and interface language became one heading.
  - **Ribbon icons are now per template, not per group.** They used to be keyed by group name, so merging groups would have left six templates sharing one icon. The distinction belongs to the template, not to its cluster. This is an interim step: every icon is still an emoji, and **seven emoji are shared across seventeen slots** (measured). The data package and brief for a designed single-colour SVG set are under `docs/design/`; the inventory is generated from the source with `node scripts/dump-ribbon-icons.mjs`, because a hand-kept list goes stale at the first new command.
- **0.36.0** — **The delivery package now generates ICN metadata files (IMF) too.** When the specification enumerates what a delivery contains, it names illustrations and **ICN metadata files** separately (Turkish MoD Integrated Product Support Data Requirements, items 2.1 and 4.1.1.4). The naming rule was already in place but nothing generated them — the last open item in the package, now closed. An IMF is an illustration's identity block: which ICN, what title, what security classification, and which file formats that ICN exists in. **The author writes no metadata by hand**; all of it is derived from the files in the folder — variants sharing one name are counted in a single record as `icnVariation`, and the **title is read from the title of the figure that references the illustration**. As with the DML, that means using what the documents say rather than keeping a parallel register. **A hand-written IMF is left alone** — the author's text always wins over a generated one. **No IMF is generated at Issue 4.1:** that generation's schema package has no `icnmetadata.xsd` (like `brdoc.xsd`, it arrived later). The illustration itself still travels in the package; only the metadata file is absent. **Measured:** the sample naval publication's 8 illustrations produced 8 IMFs and **all 8 validate against their own schema**; the titles came out as real figure titles ("Propulsion system, plan view"), not ICN codes. The DDN counts 72 of the 73 files (all but itself). Building the package a second time leaves the file count unchanged and preserves an edited IMF. In a 4.1 project the ICN is there and the IMF is not.
- **0.35.1** — **A PDF exported from Page View put a blank sheet behind every page (Web Edition only).** A one-page document printed as two, and a long one had every sheet doubled. **The cause was a unit mismatch.** In Page View the document is paginated in the client and the print engine is told only the paper size; the client sends that in **inches** (Electron's `printToPDF` contract — the desktop edition hands the same object straight to Electron and was always correct). The server's converter read those numbers as **microns** and divided by 1000, shrinking A4 to eight thousandths of a millimetre. **The real trap is that Chrome says nothing:** an impossible `@page` size raises no error, the rule is discarded and it falls back to its own default paper (measured: MediaBox 612×792 pt, US Letter). An A4 sheet is 17.6 mm taller than Letter, so each sheet spilled onto a second, near-empty page. The output was not corrupt — it was printed on the wrong paper. The ordinary (unpaginated) PDF export was unaffected: that path names the paper as the string `"A4"` and took the converter's sound branch. **Measured:** two A4 sheets — 4 pages on Letter before, **2 pages on A4 (595×842 pt)** after; the object and string forms now produce identical output, and A3 and landscape sizes come out right.
- **0.35.0** — **TIFF illustrations are now drawn: on screen and in the PDF.** 0.34.0 solved referencing and delivering a TIF but could not draw one; a box reading "cannot be previewed" stood where the figure belonged. **That box was printed into the PDF as well** — an author-facing warning inside a delivered document. Not acceptable. TIFF is now **decoded inside the page**. No browser engine draws TIFF, so the decoder is written by hand — the same reasoning as `cgm.ts` and `zip.ts`: this product is installed on air-gapped networks, where every third-party library is more code a customer's security review has to read. Decoding in the page fixes the PDF for free, because the PDF is produced from that DOM. **The specification set the scope:** the two profiles the Turkish MoD's data requirements name — **CCITT Group 4** (bi-level drawings and scans) and **LZW** (colour) — plus uncompressed and PackBits, which come free. An unsupported variant (JPEG-in-TIFF, tiled images, planar storage, 16 bits per sample) falls back to the old placeholder rather than drawing something wrong. **Measured:** **16 of 16** files produced by libtiff and Pillow decode **pixel for pixel** — including a 2400-pixel-wide image (extended makeup codes), a dithered halftone (dense horizontal mode), striped G4, LZW with a predictor, palette, greyscale and big-endian. A full A4 scan at 300 dpi takes **56 ms**, a 3-megapixel colour LZW **268 ms**. The exported PDF carries the embedded image as `/Width 211 /Height 143`, with no warning text.
- **0.34.0** — **New projects can now be started at Issue 4.1 and 5.0, and the board's button opens the wizard in one press.**
  - **All four generations can start a project.** 0.33.0 made 4.1 and 5.0 data sets *readable*, but the wizard's issue list still offered only 6 and 7, on the grounds that "the templates were written to the 6/7 content model". Measured, that did not hold: the starter set the wizard generates validates against its own schema package 10 modules out of 10 at Issue 5.0, and 9 out of 9 at Issue 4.1. All four are now listed.
  - **No business rules *document* is generated at Issue 4.1.** The one obstacle was not an element difference: the 4.1 schema package has no `brdoc.xsd` at all — that document type arrived in a later generation. So in a 4.1 project that module is neither generated with the starter set nor offered on the Module ribbon; generating it would produce a file that could never validate. The machine-checkable half of the rule book (the BREX) is still generated and still validates at 4.1; only the prose half is absent.
  - **"New project" on the board works in one press.** The button created the project but stayed on the board; the author then had to press **New Project** inside the editor as well before any modules appeared, which made the first press look broken. The board button now opens the wizard directly and the starter set is created on that press.
  - **The delivery package declared the wrong issue for older generations.** The DML and DDN generated from a 4.1 or 5.0 project announced themselves as Issue 6, which the receiving party's validation would have caught. The generation is now read from the modules' own declaration (commonest wins, so one damaged file cannot drag the package into the wrong generation), and all four generations' DML/DDN validate against their own schema package.
  - **The distribution note is placed first in the archive.** The Turkish MoD's Integrated Product Support Data Requirements (item 3) recommend that the data distribution note be the first data file in the sequence for serial media; it was previously appended last.
  - **TIF illustrations can now be used.** The Turkish MoD's data requirements list TIF among the permitted formats (CCITT/4 or LZW), yet the editor would not even let you select a `.TIF`. It now does: the file is declared in the module's DOCTYPE with the specification's own notation (`<!NOTATION tiff PUBLIC "+//ISBN 0-7923-9432-1::… Aldus/Microsoft Tagged Interchange File Format//EN">`), is recognised as a graphic by bulk import, and travels in the delivery package. **There is no preview** — no browser can draw a TIFF — so a placeholder naming the file and its format stands where the raster would be; the figure's number, title and place in the document are unaffected. Verified: a 4.1 module carrying a TIF figure validates against its schema, and the file is in the delivery archive (the DDN counts 11 files, the TIF among them).
- **0.33.2** — **The module list stayed empty when the editor opened without a file.** Entering through **Open the editor** on the project screen left the sidebar showing nothing until the author pressed **Open** as well, and the modules looked as though they had gone missing. The cause: the file list was a by-product of opening a document — `loadFile` was the only thing that populated it, and it never ran on a fileless entry. The desktop never showed this because opening a file is how you start there. The editor now knows which project it is inside and fills the list on arrival; clicking a name in that list also works with no file open, where it previously did nothing at all.
- **0.33.1** — **The unsupported-issue message named the wrong set of packages.** 0.33.0 added the Issue 4.1 and 5.0 schema packages, but validation's refusal still read "This build carries the Issue 6 and Issue 7 schema packages only" — telling the reader that a supported issue was not supported. The list is no longer written into the sentence by hand; it is read from the bundled schema folders, so a package that is added can no longer fall outside the message. The same staleness had been fixed in the guide footer in 0.33.0; this was its overlooked twin.
- **0.33.0** — **Delivery packages, and Issue 4.1 and 5.0 data sets can be opened.** Two substantial gaps closed: the delivery package procurement specifications name explicitly, and the older-generation CSDBs you already hold.
  - **Delivery package (DML + DDN + a flat archive).** A specification does not ask for "the files"; it asks for a *declared* delivery: a **DML** (data management list) enumerating what the package holds, a **DDN** (data despatch note) saying who sent it to whom and when, and a flat file set with no directory structure. Until now the project folder was copied by hand. A **Delivery package…** button on the project screen now produces all three. The identity fields (model code, sender, security classification) are derived from the modules' own ident blocks — the files are the single source of truth rather than a parallel register that can drift. Names follow the `TYPE-CONTROLNUMBER.FORMAT` upper-case convention the specification expects.
  - **The archive writer was written by hand rather than pulled in.** Compression is Node's built-in zlib; the scope is deliberately narrow (no ZIP64, no encryption, no directory entries) because the specification wants a flat file set anyway. The reasoning matches the ZIP *reader* on the import side: this product is installed on air-gapped defence networks, and an archive library is a great deal of third-party code to audit for what amounts to writing a directory table.
  - **Issue 4.1 and 5.0 data sets open and validate.** The schema package and default BREX for both generations are now bundled. A 4.1 CSDB is validated against its own schemas; previously it was quietly treated as Issue 6 and produced a meaningless wall of errors. Exercised against the real ASD/AIA sample sets: 103 of 103 files passed for 4.1, and 100 of 101 for 5.0 — the one exception declares Issue 4.2 and is now refused by name.
  - **New projects are still created only at Issue 6 or 7.** This is deliberate: the templates are written to the 6/7 content model, and 4.1's vocabulary is 36 elements narrower. The product *reads and validates* the older generations but does not *author* in them.
  - **Business rule decision points follow the generation.** The roll is not stable: 4.1 defines 552 decision points, 5.0 defines 426, 6 and 7 define 432. The panel now shows the roll belonging to the issue the document declares — showing the wrong one asked authors questions the specification never posed.
  - **An unsupported issue is now named.** Opening a document from Issue 4.0 or earlier reports *"Unsupported S1000D issue: 4.0. This build carries the Issue 4.1, 5.0, 6 and 7 schema packages only."* instead of validating against the wrong package and producing hundreds of meaningless schema errors.
- **0.32.0** — **The web edition now has the embedded offline translation.** Machine translation used to be desktop-only; on the server the panel opened and the translation memory worked, but it said "no embedded translation model in this installation". The same engine now runs on the server: English to Turkish, fully offline, no text leaves the machine.
  - **The engine runs in a process of its own.** On the desktop the only person waiting for a translation is the author who asked for it; on a shared server, CPU spent translating is CPU taken from everyone else's saves, validations and PDFs. So translation runs as a **child process** of the server: the event loop is never blocked, it only ever waits on a message. Requests queue — the engine holds one worker, and running several at once would multiply memory without multiplying throughput. The model stays loaded between requests (first translation ~0.3 s, later ones instant) and the process exits after ten idle minutes to give the memory back.
  - **One engine, both editions.** `electron/mt.cjs` became `shared/mt.cjs` and its Electron dependency was made optional. A second copy would have meant two `LocalBacking` classes and two timeout policies drifting apart — the shape that has bitten this project before.
  - **The package grew by 25 MB, and the old reasoning was wrong.** The build script excluded the model as "200 MB of dead weight". Measured: 25 MB — a 5 MB engine and one 20 MB language pair. That figure dated from 0.27.0, when German and French shipped too; both were reverted in 0.27.1 and the reasoning was never revisited.
  - At most 500 segments per request; the panel already sends a module at a time. When the engine fails the response is `200` and **carries the reason** — a 500 reaches the panel as "MT failed" and explains nothing.
  - The server logs which pairs are installed at start-up.
- **0.31.3** — **An invisible space in the password is now named.** The sign-in screen rejected a correct user name and password and said only "Wrong user name or password". The cause was a space at the start or end of the password — the kind a copy-and-paste picks up. The server normalises the user name (case and surrounding spaces do not matter) but takes the password verbatim, which is right: a space is a legal password character. What was wrong is that the screen never said so, and every character in that field is rendered as a dot, so nobody could see it. When sign-in fails and the password has leading or trailing whitespace, the screen now names it; the same applies to the "current password" field on the forced password change. Silently trimming the password is not the fix — it would lock out anyone whose password really ends in a space. Found on a real installation, after half an hour lost to it. This is the third defect of the same shape on this screen in three releases: a dead Save button (0.25.3), "your session has ended" for a wrong password (0.30.1), and this one. Each time the screen refused without saying what to look at.
- **0.31.2** — **Uninstalling no longer deletes the CSDB, under any circumstances.** The uninstaller used to ask "delete the data folder?" — default No, and a silent uninstall never deleted it. That was still not enough: the delete used `RMDir /r`, which does **not** go through the Recycle Bin, so one wrong click destroyed every project, data module, version and account with no way back. It happened on a real machine on 2026-09-09 to someone who was simply reinstalling, and the loss was only noticed half an hour later, when the server came up on an empty CSDB. The uninstaller now prints where the data folder is and **leaves it exactly as it is**; anyone who really wants it gone deletes the folder themselves, which puts it in the Recycle Bin and can be undone. No convenience justifies an installer that can destroy a publication irreversibly in one click.
  - **The server now says when it opens an empty CSDB.** If the project count is zero the log adds: "if you expected to find some, the server is reading a different directory than you think — check `S1000D_DATA_DIR`". That was the signal that would have caught the incident above in thirty seconds instead of thirty minutes, and it was missing.
- **0.31.1** — **Certificate generation never worked on a Turkish Windows.** The new TLS step in 0.31.0 failed silently during installation, leaving the server without TLS, and running *TLS Sertifikasi* from the Start menu printed a red PowerShell error: `An object at the specified path C:\Users\GKHAN~1 does not exist`. The cause was that the script put its scratch files in the user's TEMP folder: on a Turkish Windows the profile name collapses to an 8.3 short form (`GKHAN~1`), and PowerShell reads a tilde *inside* a path segment as a home-directory shorthand — even under `-LiteralPath`. Worse, that is a parameter-binding failure, which `-ErrorAction SilentlyContinue` does not suppress, so a line that only did *cleanup* killed the whole run before a certificate was ever written. Scratch files now live in the `tls` folder the script already creates and locks to administrators, and deletions go through .NET so a failed cleanup cannot end the run. Reported from a real Windows installation.
- **0.31.0** — **The installer now produces the TLS certificate itself.** Until now the server was installed speaking plain HTTP and waited for someone to read the guide and prepare a certificate by hand; in practice nobody does that on install day, and passwords went over the wire in the clear. The setup wizard has a third question: **TLS — generate a certificate (default) / I will supply one later.** With generate, the installer prepares a certificate carrying the machine name, the domain name if there is one, and the machine's IP addresses in its Subject Alternative Name, fills in `settings.bat` itself, and the server speaks `https` from its first start. If the machine already holds a usable certificate — a domain member usually does — that one is used instead of generating a self-signed one, which would have been a downgrade there. For a self-signed certificate, the command to export its root for distribution is printed on screen. **The installer checks that the server's own Node can actually open the certificate before writing it into settings.bat**: if it cannot, settings are left untouched, because otherwise the server would refuse to start after the installer window had closed.
  - A Start-menu shortcut, *TLS Sertifikasi*, generates or renews the certificate later. It never overwrites TLS settings that are already there, so an upgrade leaves the operator's own certificate alone.
  - **The access check now lists addresses with the right scheme**: `https://` when TLS is on, and when it is off it points at the one-step fix instead of repeating a warning.
  - The `tls` folder holding the private key is opened to Administrators and SYSTEM only.
  - Found by measurement: passing an IP address to `New-SelfSignedCertificate -DnsName` writes it as a *DNS* entry rather than a real IP SAN, so a browser reaching the server by address would call the certificate invalid. The SAN is now encoded by hand with `-TextExtension`.
- **0.30.1** — **A wrong password said "your session has ended".** Anyone who mistyped their user name or password on the sign-in screen was shown *"Your session has ended."* instead of the server's own *"Wrong user name or password."*, and the same happened on the forced password change that follows a first sign-in when the current password was wrong. The message sent people looking for a cookie or session fault instead of a typo — on a real installation it cost an administrator half a day. The cause: the client's single HTTP layer treated *every* 401 as a dead session and overwrote the server's explanation without reading it. But the sign-in and password-verification endpoints are where a session is *obtained*; a 401 there means "what you just typed is wrong". Those two are now excluded, and both forms name the field that was wrong, in the interface language.
  - **Windows Web Edition: the "S1000D Editor" Start-menu shortcut opened a blank page.** It chose `https://` only when a `TLS_KEY`+`TLS_CERT` pair was configured, so on servers set up with `TLS_PFX` — the form the guide recommends on Windows — it tried `http://` and hit nothing. It now recognises both certificate forms.
  - **A non-existent account could be "added" to a project.** With a mistyped user name the server wrote it into the member list instead of refusing: the member count went up and a row appeared, while the real person still could not see the project. A missing field produced a member literally named `undefined`. Adding a member now checks that the account exists. Removal is deliberately left unchecked, so a stale membership belonging to a deleted account can still be taken out.
- **0.30.0** — **A path-separator bug on Windows: "Create translation" did nothing.** `dirname()` understood only `/`, and on Windows the OS dialog returns `C:\...\module.XML`, so it **took the whole file path for the directory**. The translation sibling was then written into a folder that does not exist, the write threw inside the main process, the error reached nowhere, and the author saw only "I pressed the button and the page did not change". It never appeared on macOS.
  - `dirname()` and `basename()` now understand both separators; measured on UNC and mixed paths as well
  - The same helpers are used in **25 places**: the sibling file list, the project BREX during validation, the graphics directory, the translation memory, bulk operations, front matter reports. All of them were affected on Windows
  - **The silence was fixed too:** the three early returns in the create path and a failing write now appear in the status bar with their reason, and the main process reports which file and which folder instead of a bare `ENOENT`
  - **The target language menu is gone when there is only one.** With the source language excluded, a single remaining target is now stated as text rather than offered as a one-item menu that asks a question already answered
  - **The web server edition is now called "S1000D Web Edition".** "Web Server" read as a separate product, when it is a second way of installing the same editor. The installer file, the Start menu folder, the scheduled task and the firewall rule all changed name. An upgrade **removes the old task and rule** — left behind, they would run a second supervisor against the same data directory. The registry keys keep the old name so Programs and Features shows one entry, not two
  - The access check listed the same fix twice (a Public network profile also raised the rule-coverage warning); one cause now produces one line
- **0.29.1** — **An access check at the end of the install.** The web server install could succeed at every visible step and still be unreachable from outside: the service runs, the port listens, the firewall rule is there — and authors get a timeout. The usual cause is Windows putting the network in the **Public** profile, which the rule deliberately does not cover, so packets are dropped silently. Nothing said so anywhere, and on a real Windows install it took five rounds to find.
  - A new `check-access.ps1` tests **every condition** that has to hold: is the server answering, is it listening on `0.0.0.0` or only on `127.0.0.1`, what profile is the network in, is there an enabled allow rule, and is there a **`node.exe` block rule overriding it** (block rules beat allow rules, and produce exactly the same silent symptom)
  - It names the condition that fails, with the command that fixes it; when they all hold it lists the addresses to hand to the authors
  - It runs once by itself at the end of the install, so the answer arrives while the operator is still at the machine. A **Erisim Denetimi** shortcut was added to the Start menu, for re-running it after a port change
  - A **User Guide (English)** shortcut was added to the Start menu as well
- **0.29.0** — **Import now compares content.** The plan used to look only at the file *name*: if something of that name was there it said "already there", without saying whether it was the same or different. Migrating from several desktops onto one server, that lost the handful of modules genuinely needing a decision among hundreds of identical ones.
  - "Already there" split in two: **Identical, will be skipped** (writing it would change nothing, so it is skipped even with the overwrite box on, and no pointless version history entry is made) and **Differs**, with the size and modification date of the file in the project printed under it
  - XML comparison normalises line endings, so a data set does not read as "changed" merely because it passed through a Windows machine
  - A `GET /api/digests` endpoint was added — one request per project rather than one per colliding file, so the plan still appears at once across hundreds of collisions
  - *Measured:* the client and server digests agree for the same file (checked separately for byte-identical, line-endings-only, genuinely changed and binary files); the plan returns `identical / exists / new` with overwrite off and `identical / overwrite / new` with it on
- **0.28.3** — **The user guide is now in English as well.** All 21 sections are translated; the guide opens in whichever language the interface is set to, on the desktop and in the web edition alike, and a guide window left open from before a language change reloads rather than showing the other language. The release history inside the English guide is generated from this file, and the build fails if it and the Turkish list stop describing the same versions.
  - Two stale passages were found while translating and corrected in both languages: the first sign-in still told the operator to read the admin password out of the server log (it has been `admin`/`admin` since 0.24.1), and the PDF section still described Page View as a separate edition (it was folded into the single package in 0.21.0)
- **0.28.2** — **The release history is now inside the guide** (Guide → Releases): you can see what changed in the package you installed from the application itself. The list is not written by hand — its source is `SURUMLER.md` in the repository, and the copy in the guide is generated from it on every build (`scripts/sync-changelog.mjs`), so the two cannot drift. The same script stamps the version strings in the guide from `package.json`: the header badge, the colophon and the package file names in the installation table. All three had been typed by hand and all three had been stuck at 0.25.1 — a guide that disagrees with the package it shipped in is worse than one that says nothing
- **0.28.1** — **The translation engine hung silently.** If its worker failed to start, the engine reported the error only to `onerror` (a `console.error` that goes nowhere in a packaged application) and returned, leaving the queued work in the queue and every `translate()` promise unsettled — from the outside, a button that said "Translating…" for ever. Measured: unpatched, no result and no error in 25 seconds; patched, an error naming the exact cause in **23 ms**. Three fixes: (1) a start-up failure now rejects everything queued, (2) the worker resolves its own location with `pathToFileURL` — the old `"file://" + __filename` survived `C:\…` by luck but broke on a **UNC path** (`\\server\share\…`), where the wasm could not be found, (3) the engine's error is surfaced on screen, and a scaled timeout (30s + 5s per segment, at most 15 minutes) turns an unknown hang into a message too. The error box is selectable and scrollable, so the text can be copied and reported
- **0.28.0** — **Zoom status bar** along the bottom edge of the window: 📄 Page View · － · the current percentage · ＋ · ⤢ Fit page width. The bar runs the ribbon's *own* commands rather than copies of them, so the two places cannot disagree about pressed state or about the "available in page view" reason. The percentage is the one **applied**: with fit-to-width chosen it shows the factor the window worked out; with page view off nothing is scaled, so it reads `—`. The ribbon label changed from "Fit width" to **"Fit page width"**
- **0.27.1** — The German and French models were **removed**; embedded translation is English→Turkish again. The target language list is limited to Turkish and English too: these publications are produced in those two, and keeping an unused language brought nothing but the risk of creating a sibling module in the wrong one. The installer went from 147 MB to 94 MB. The discovery machinery stayed — adding a language back is uncommenting one line in `scripts/fetch-mt-models.mjs`
- **0.27.0** — **Creating a project in the web edition split it in two.** *New project* on the board opens an empty CSDB (name and members), while the wizard in the editor created **a new server project every time**; the result was two projects, one with the people and one with the modules. The wizard now writes the modules **into the project you are in** when it is empty; when it already has modules it asks for a new project's name as before and never overwrites.
  - **German and French** were added to embedded translation (reverted in 0.27.1). The models are Mozilla's Firefox Translations "tiny" models and now come from the **Remote Settings** collection Firefox itself downloads — the GitHub repository is unusable, its Git LFS objects having been purged. The new `scripts/fetch-mt-models.mjs` verifies every file against the SHA-256 the registry publishes; it is the only part of the project that touches the network, and only at build time. `electron/mt.cjs` became pair-agnostic: it finds the installed pairs by scanning `manifest.json` files on disk, and the panel asks the application for the list
- **0.26.0** — **Printed labels did not follow the module's language.** The `lbl(en, tr)` mechanism existed but was wired into only 15 places, all of them in the BREX viewer; the body of the document (WARNING/CAUTION/NOTE, Fig, Table, Procedure, Preliminary requirements, every table column heading, cross-reference labels, Table of contents, "None") was hard-coded English. Some 50 call sites were connected. **The label language is the module's own** (`dmIdent > language`), not the interface's: an English data module must still say WARNING in a Turkish interface, because it goes into an English book and ASD-STE100 fixes the word; in a bilingual publication each module prints in its own language. A file that declares no language falls back to the interface language.
  - In the translation panel the auto-translate button **vanished silently** when a language with no model was chosen; a line now stands in its place naming the language and listing the installed pairs
- **0.25.3** — **The Save button on the password-change screen was dead.** It was disabled until the new password reached eight characters, but nothing on screen said so — a dead end on the sign-in path, with no way forward. The same pattern was in the administrator's "Reset password" dialog. The button is now always enabled, validation happens on submit and **names the rule that failed**, and the rule is stated under the field as well. `MIN_PASSWORD` moved to one place, holding the same number the server enforces
- **0.25.2** — The password recovery instruction **did not match cmd syntax**: the readme said to add the line `S1000D_RESET_ADMIN=1` to `settings.bat`, but that file is a batch script — a bare line is parsed as a command, skipped, and the variable is never set. The correct form is `set "S1000D_RESET_ADMIN=1"`. `settings.bat` now carries the line **already there, commented out**, so the instruction became "delete the `rem`" rather than "type this correctly"; the readme was split into five steps and gained a version warning and a last resort (renaming `users.json`). Note: an upgrade preserves the existing `settings.bat` and writes the new one as `settings.bat.new`, so on an older installation the line must still be typed by hand
- **0.25.1** — **The "first run" hint on the sign-in screen was misleading.** It appeared whenever *any* account still had an unchanged password, whereas `admin`/`admin` only holds on a system installed **new** at 0.24.1 or later. The result: anyone upgrading an older installation (a random password, never signed in), and every user whose password an administrator had reset, was told "the password is admin" — one stayed locked out, the other was handed an invalid credential. The hint now checks whether the account really carries the **published default password**, and disappears by itself once it is changed.
  - **A recovery route was added:** set `S1000D_RESET_ADMIN=1` in the service environment and restart, and the administrator password returns to the default and must be changed at the next sign-in. It takes the same access as editing the data directory by hand (it grants no new capability), but unlike deleting `users.json` it destroys no account, project or version history
- **0.25.0** — **S1000D Issue 7 support.** The issue is now a *project* property: a CSDB is at one issue, and S1000D asks this as a decision point of its own (BRDP-S1-00003). The application carries both issues side by side — existing Issue 6 data sets keep working as they are, and new projects start at Issue 7. *Measured:* Issue 7 is a superset of Issue 6 — 22 elements added, none removed, and no enumeration this tool uses changed; all 55 modules the application produces are valid Issue 7 unchanged. The BRDP decision points are 432 in both issues and identical in content, so the decision index stayed common
  - Validation reads **which issue** a document is at from its own `noNamespaceSchemaLocation` and checks it against that issue's schema package (desktop and web server, one rule). Older samples declaring Issue 5.0 are met with the Issue 6 package rather than rejected
  - The default BREX is chosen by issue (Issue 6: 255 rules, Issue 7: 263)
  - An **S1000D issue** field in the New Project wizard; every module, ACT, BREX and publication module produced declares that issue's schema
  - **What Issue 7 brings is now printed:** the `checkListItemData` values of checklist items (project-defined codes) and `taskDuration` times were silently dropped instead of reaching the page
  - XML files that are not data modules (ICN metadata files, root `<icnMetadataFile>`) are now met by saying what they are, instead of a blank page and an unintelligible error
  - The product name was simplified to **S1000D Editor** (it was "S1000D Issue 6 XML to PDF Editor"; both the issue number and the long description left the name)
- **0.24.1** — **First-run password**: in the web edition the first `admin` account is created with a fixed **`admin` / `admin`** rather than a random password written to the console — you no longer have to read the server log before you can start. In exchange, the forced change is enforced **on the server**: an account signed in with that password cannot call any endpoint but `/api/me`, `/api/me/password` and sign-out until it changes — everything else returns HTTP 403. That restriction used to live only on the browser screen, so any client that could reach the port could step around it; that is the change that makes a fixed default password safe. `S1000D_ADMIN_PASSWORD` still works as before — given it, the first account is created with it and no change is demanded. The sign-in screen gained a hint explaining this on the first run
- **0.24.0** — **Applicability**: S1000D's answer to one piece of content being used on several platforms now has its counterpart in the tool — a radar can be on a frigate and a corvette without the content being copied.
  - An **ACT editor** (`🎯 Applicability table`, with the ACT module open): defines the product attributes — identity, display name, description and value set. Leave the value list empty and the attribute becomes free text (a hull number, say). The XML it writes was validated against the real `appliccrossreftable.xsd`
  - **Filtering a publication by platform**: with a publication open, choose a platform in the bar at the bottom; modules that do not belong stay out of the book, and blocks inside a module are removed too. The cover of a filtered publication is **stamped** with the platform it was printed for — the only thing that tells two books from the same PMC apart
  - The evaluator supports `assert`, nested `evaluate` (and/or), `|` lists and numeric `~` ranges. **Nothing uncertain is ever removed**: an attribute you did not choose, a `condition` expression or a non-numeric range *leaves* the content in and is counted in the status bar. Silently dropping a warning because of an expression that was not understood is far more dangerous than leaving extra content in
  - Files on disk never change; filtering applies to the compiled publication only
  - 31 unit tests were run against the real Bike Data Set corpus; the filter was verified not by a count but by **which modules entered the book** (BR01 → 041A, SS-11 → 341A, BR02 → 251A)
- **0.23.5** — **Deleting a project** (web edition): a delete action on the cards of the Projects page, visible only to a project administrator. Because it destroys a CSDB, it has three steps: how many modules and files would be lost is shown, the button stays shut until the **project name is typed by hand** (checked on the server too, not only in the browser), and the default behaviour is **reversible** — the folder is moved under `data/.deleted/<project>-<timestamp>/`. For controlled documents that must not stay on the server there is an explicitly marked permanent delete. Either way the open locks in the project are released and the action is written to the audit trail (`project.delete` / `project.purge`)
- **0.23.4** — **TLS setup**: a certificate can now also be given as **PFX/PKCS#12** with `TLS_PFX` + `TLS_PFX_PASSWORD` (previously only a PEM pair). Since both an internal CA request and `New-SelfSignedCertificate` produce a `.pfx` on Windows, this removes the openssl conversion step entirely on a closed server. Neither failure route is silent any more: if the certificate cannot be read the server writes the reason and stops **rather than falling back to HTTP**, and a `.pfx` with legacy encryption (RC2/TripleDES — still offered by the Windows wizard) produces an explanation saying "re-export with AES-256" instead of a raw stack trace. The guide gained a TLS section covering an internal CA, self-signed certificates, why SAN is required, and distributing the root to clients
- **0.23.3** — **Importing a data set** (web edition): *Import…* on the project board brings a CSDB you already have into the project — multiple file selection or a ZIP archive. The ZIP is opened in the browser (a dependency-free reader plus `DecompressionStream`), sub-folders are flattened and `__MACOSX`/`.DS_Store` leftovers discarded. A row-by-row plan is shown before anything is written: each module's DMC, title and schema, which file will be added, which is already there, which someone else has locked, and the ICNs the modules refer to that are missing. The previous state of every overwritten file enters version history; the lock applies here too. A side fix: version snapshots were read and written as UTF-8, which corrupted the previous version of a graphic — they are copied byte for byte now
- **0.23.2** — The application opened with a "trial copy — extracting the source code or reverse engineering is prohibited" notice screen (`TermsNotice.tsx`). It contradicted the nature and the decisions of the project itself (a non-commercial, in-house tool) and, on top of that, was never recorded as accepted and so appeared again and again. The screen was removed entirely along with its i18n strings, on the desktop and in the web edition
- **0.23.1** — **User administration module** (web edition): creating accounts one at a time or from a previewed bulk list, editing role/unit/e-mail, resetting passwords (the generated password is shown once and changed at first sign-in), ending sessions, disabling, deleting, managing project memberships from the person's side, CSV export. The server side moved into its own module (`server/users.mjs`): removing an account touches `users.json`, `projects.json`, `sessions.json` and the in-memory locks at once, and a half-finished delete would leave the list untrue to reality. An administrator cannot demote, disable or delete their own account — the one guard that keeps an installation from ending up with no administrator. Also fixed: the primary button in modal dialogs rendered white on white (a CSS ordering collision)
- **0.22.1** — Fixes to the web edition after live testing on Windows:
  - **Critical:** a **silent uninstall** of the web server (`Uninstall.exe /S`) deleted the data directory without asking — an NSIS `MessageBox` with no `/SD` answers "Yes" automatically in silent mode. `/SD IDNO` was added; a silent uninstall now preserves the data (it is deleted only in an interactive uninstall, when "Yes" is explicitly chosen)
  - **The New Project wizard in the web editor did not open the module it had created** (the lock was taken but the preview stayed blank): creating the project changed the shell's `project` state, which remounted the editor through its **React key**, leaving the wizard's in-flight `openCreated` on the old, unmounted component. The editor key is now tied only to the open file, not to the project id
  - The web server installer now writes `$SMPROGRAMS` in the **all users** context (`SetShellVarContext all`) — previously the shortcuts reached only the Start menu of the account that ran setup
  - A `settings.bat.new` left by an upgrade is now removed on uninstall; version and product information were added to the installer's file properties; an em dash in the first-run log rendered as mojibake in Notepad and was replaced with a hyphen
  - Windows usually marks new networks with the **Public** profile; the firewall rule is deliberately opened only for the Domain and Private profiles (opening Public would be a security regression in a defence context) — the guide gained a warning to check that profile
- **0.22.0** — **Web (multi-user) edition**: the same editor on a server in your own network. Accounts and roles, project (CSDB) membership, a per-module check-out lock and live presence, version history, review status, an audit trail. The server uses no additional npm dependency, and the project folder layout stays identical to the desktop edition. The editor code did not change
- **0.21.1** — Interface touch-ups: the ribbon's **Find command** icon became a magnifier (🔍); the shortcut badge (`Ctrl+K`) on the quick-search button at the top right is shown **on macOS only** — on Windows the button simply reads "Find command" (the shortcut itself keeps working)
- **0.21.0** — **Folded into a single package**: the separate *Page View* edition was removed and A4 page view is in the normal package (`src/edition.ts` and the `VITE_EDITION` split were deleted). The page commands are in View › **Page** in every installation; page view is **off by default**, so the behaviour of existing installations does not change, and turning it on is remembered
- **0.20.0** — **Context menu in the document map**: right-click a branch of the tree for *Change…* and *Delete*. The change dialog adds, alters and removes an element's attributes and edits the text of an element that has no children — so fields the preview never shows (dmIdent, security, applic) can be edited without touching the XML by hand. An invalid or duplicate attribute name is refused without the document being touched. Delete asks for confirmation and protects the root element; both operations can be undone and the DOCTYPE (graphic entity declarations) is preserved. Also: **document-bound panels now close when the file changes** — the 022 (BREX rule editor), 024 (business rule decisions), IPD part/BOM and translation panels no longer stay open over a document they no longer recognise once you leave their module (their own ribbon commands had already gone inactive). The New Module and Bulk Import panels are project-bound and stay open. When a decision cannot be saved, the BREX rule is no longer written either. **The application menu was removed entirely on Windows**: the menu was already hidden (the ribbon is the command surface), but a hidden menu bar took focus on Alt and then swallowed the letters typed after it — on a Turkish-Q keyboard the `@` of an XPath is typed with AltGr (= Ctrl+Alt), so typing could become impossible in the BREX rule editor. The shortcuts the menu carried (Ctrl+O/S/P, F1) are now in the renderer on Windows and Linux; the macOS menu is unchanged
- **0.19.0** — **Page view edition** (a separate package). The preview is laid out as real A4 sheets rather than a continuous column: margins, headers, footers and real page numbers on screen. The same paginated document is handed to `printToPDF` with zero margin and one sheet per page, so **the page on screen is the page in the PDF** (the engine has nothing left to split). A block that cannot be split and is longer than a page is not clipped but scaled down to fit, and reported in the status bar. The sheets re-flow while editing continues, with the caret left where it was
- **0.18.0** — **Bulk import** (Home › 📊 Bulk Import): CSV is read from the planning sheets in the `toplu-islem/` folder and the format recognised from the headers. The new-project CSV produces modules through the same code path as the New Module wizard (never overwriting; ACT/BREX included; a graphic column adds the figure and the DOCTYPE declaration), and the update CSV revises existing modules **at text level** (only the changed lines, DOCTYPE preserved), with the issue type derived from the action. The panel shows the row-by-row plan, and why a row will be skipped, before anything is applied
- **0.17.0** — Folded into a single distribution: embedded offline translation is now part of the standard package (the separate "MT" edition was removed). **Figure legends** are printed: a `legend/definitionList` inside a `figure` appears under the illustration in the preview and the PDF as a number → definition list — in a multilingual publication the illustration stays common while the legend is translated with the module. Eight technical illustrations (ICN/SVG) were added to the sample naval data set
- **0.16.0-MT** — A separate **MT edition**: embedded offline English→Turkish translation (Bergamot/Marian, WASM; a Mozilla tiny model, hash-verified). The translation panel gained 🤖 "Auto-translate blanks", filling empty segments with suggestions (memory ⟳ and suggestion 🤖 carry separate badges); the author approves every line. The engine runs in the main process, there is no network, and the renderer CSP is unchanged. The standard package was not affected
- **0.16.0** — **Translation module** (Publication tab › 🌐 Translate): the source module's text is extracted segment by segment, the author enters each target, and a **parallel-language module** conforming to S1000D is produced (the same DMC, a sibling file such as `_TR-TR.XML`). Codes, cross-references and inline formatting are preserved and only the words change. There is **no machine translation** — the tool stays offline; a per-project **translation memory** (`s1000d-tm.json`) fills repeated text automatically. Target languages: tr-TR, en-US, en-GB
- **0.15.0** — Security hardening: the graphic injection route was closed (the ident and file name are handled through DOM APIs and no longer reach `innerHTML` unescaped; only `data:` image/video/audio is accepted), and a Content-Security-Policy was added (preview/PDF: scripts and network forbidden; main window: `connect-src 'none'` blocks data leaving the application)
- **0.14.0** — The business rule decisions panel now writes to the file directly from its Save/Update/Remove buttons (no separate ribbon Save); the duplicated contextual "Business Rules" tab was removed (decisions live only on the BREX tab); an empty decision raises a "pending" warning
- **0.13.0** — **Project profile**: the product type (land/sea, air, generic) and the extra capabilities in use (IETP, training content, service bulletins, BDAR) are chosen, and out-of-scope module templates and decision point groups are hidden from the interface (432 → 396 decisions in a land/sea profile). The ribbon was reorganised: Visual/XML moved to **View**, New Project and Properties to **Home**, and the **Review** tab became **BREX** with the rule editor moved into it
- **0.12.0** — The interface became fully bilingual: every panel, menu, status message and template name in English and Turkish. The XML content produced follows **the document's** language (no Turkish text in an en-US module), and so do the renderer's labels. New projects produce LOEP/LOEDM/Highlights/Contents from the real module list; a duplicated title on the cover page was removed; the date column in front matter tables no longer wraps and the page columns are centred
- **0.11.0** — The application was named **S1000D Issue 6 XML to PDF Editor**; the ribbon's Module tab lists every module type; operations moved into panels docked to the right (new module, BOM import, BREX, business rule decisions); the sidebar can be closed; DMC codes became clickable links; new projects come with front matter modules (cover/Contents/LOEP/LOEDM/Highlights) and a business rules module; a **business rule decision point (BRDP)** panel — the specification's 432 decision points embedded, with a decision optionally becoming a BREX rule; zoning (Ch. 3.4) zone and access point references and templates; **English became the default interface language, with Turkish an option**
- **0.10.1** — BREX modules now show their rules on the page (a path / status / description / permitted values table, non-context rules included); a rule added in the editor appears on the page at once
- **0.10.0** — Ribbon interface: commands were divided into Home/Insert/Module/ Publication/Review/View tabs, with context tabs opening in IPD and BREX modules. The command palette remained as a secondary quick access
- **0.9.0** — Command palette (⌘K / Ctrl+K): every command in one search box, accent- insensitive Turkish search, shortcut display, and a reason for commands that are unavailable
- **0.8.0** — Front matter report generation: Contents, LOEP, LOEDM and Highlights data modules are derived from the publication module, with page counts measured from the real layout. Front matter tables print exactly as in the specification's examples; change record (CR) and technical standard record (TSR) templates were added
- **0.7.1** — Maintenance planning limit codes are shown with their readable meanings (`th06`→Days, `lt07`→Functional check; the th01–th39 and lt01–lt07 tables were extracted from the official S1000D default BREX)
- **0.7.0** — **Chapter 3 information sets**: fault report/isolation (fault), maintenance planning and time limits (schedul), crew information (crew), checklist, common information repository (comrep), wiring data (wrngdata), process and service bulletin (sb) modules are now laid out on the page; the first seven have a schema-valid generation template in the wizard (17 templates in total). The sample corpus grew to 106 modules
- **0.6.1** — **Fix:** on Windows and Linux an extra native "File/Edit/View/Help" menu bar appeared inside the window (on macOS that menu is in the system bar and never shows) — it is hidden now, and opens with Alt if needed. Also, Consolas was added to the monospace font stack used in the interface chrome (document map, BREX editor, IPD tables) as the fallback for SF Mono/Menlo, which do not exist on Windows
- **0.6.0** — **Module properties panel** (security classification, issue number/type/date, responsible company, applicability text, titles) and **10 module templates**: descriptive (040/041), removal/installation/servicing/inspection procedures (520/720/210/340), IPD (941), cover/Contents/LOEDM (001/009/002) — each with a content skeleton suited to its job, all passing XSD validation
- **0.5.0** — **Publication style**: page size, orientation and margins, tokenised header and footer (`{dmc}` `{security}` `{page}` …), typeface and type scale managed in one place; four presets, live preview, saved into the project as `s1000d-style.json`. **Fix:** choosing a ready-made path in the BREX editor silently changed the selected rule's path instead of adding a new rule
- **0.4.0** — The CSV import window opens by itself when a parts catalogue module is created; with a template row present the default becomes "replace the list". **Fix:** part rows carrying an SMR code produced XML that broke the schema — `ipd.xsd` makes the `service` element mandatory alongside SMR; the service code is now asked for and the import is blocked without it
- **0.3.2** — Panel widths became draggable: splitters between the sidebar and the content, and (in XML mode) between the editor and the preview; double click resets, arrow keys fine-tune, and the settings survive the next launch
- **0.3.1** — The new module wizard was rescued from a dead end: the technical name and info name arrive pre-filled by type, a disabled create button says why, and the selected type is marked with ✓. A location code sticking at `D` after choosing IPD and then switching type was fixed
- **0.3.0** — Adding a figure (copies the graphic file into the module, adds the `NOTATION`/`ENTITY` declarations to the DOCTYPE and creates the DOCTYPE if needed), visible text formatting buttons (B/I/U/sub/superscript), and a crash fix for when `window.api` is absent
- **0.2.0** — New project and new module wizards, IPD (parts catalogue) support, IPD row insertion and BOM/CSV import, the BREX rule editor, the application menu and the embedded user guide
- **0.1.0** — WYSIWYG editor, descriptive/procedural/front matter/PM support, XSD + BREX validation, a CGM decoder, hotspots and multimedia, PDF and publication output
