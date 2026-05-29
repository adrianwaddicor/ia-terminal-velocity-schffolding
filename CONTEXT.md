# IB Physics IA Writing Tool — Context & Aims

## 1. Pedagogical Goals

This tool is designed for IB Physics teachers to support students writing their Internal Assessment under the **post-2025 syllabus** (first assessment 2025). The driving principles are:

- **Reduce cognitive load** — the IA is a large, open-ended task. Chunking it into guided prompts (each targeting a specific rubric point) lets students focus on one piece at a time.
- **Preserve originality** — prompts are open-ended questions, not fill-in-the-blank templates. Every student still generates their own research question, methodology, data, and evaluation. The tool scaffolds *how* to answer, not *what* to write.
- **Embed the IB language** — prompts use IB command terms ("state," "explain," "justify," "compare") and the language of the assessment criteria so students internalise what the marker is looking for.
- **Target top band** — each prompt is worded to elicit the level of detail and specificity required for marks 5–6 in every criterion. Hints and warnings steer students away from common pitfalls that cap scores at 3–4.

## 2. IB Physics IA Structure (post-2025)

From May 2025 onward, the IB Physics IA is assessed against **four criteria** (24 marks total):

| Criterion | Marks | Sections in this tool |
|---|---|---|
| **Research Design** | 6 | 1.1 Research question in context, 1.2 Methodological considerations, 1.3 Methodology (reproducibility) |
| **Data Analysis** | 6 | 2.1 Raw data table, 2.2 Processing and uncertainties |
| **Conclusion** | 6 | 3.1 Answer the research question, 3.2 Comparison to accepted scientific context |
| **Evaluation** | 6 | 4.1 Weaknesses and limitations, 4.2 Improvements |

Each section is further divided into subsections that map to specific prompts in the form.

## 3. Technical Architecture

**Deployment model:** Fully client-side, single HTML file. No server, no database, no installs.

| Requirement | Approach |
|---|---|
| File format | One self-contained `.html` file — distribute via email, USB, or LMS |
| Word document generation | [Docx.js](https://docx.js.org/) + [FileSaver.js](https://github.com/eligrey/FileSaver.js/) loaded from CDN |
| Image handling | FileReader API → base64 data URIs → embedded directly into `.docx` |
| Auto-save | All text and image references persisted to `localStorage` on every input event |
| Progress tracking | Computed per-section as fields are filled (non-empty text / image uploaded) |
| Offline capability | After first load (CDN files cached), works without internet |
| Font embedding | Docx.js supports specifying fonts; document uses standard fonts available on all systems |

### 3.1 Why client-side only?

- Students can work on school computers, personal laptops, or Chromebooks without IT approval.
- No data leaves the student's machine — no privacy concerns.
- Zero deployment cost; just hand out the file.

## 4. Form Data Model

### 4.1 Structure

The form collects text and images for each subsection and assembles them into a Word document at generation time.

### Research Design

| ID | Subsection | Type | Word doc placement |
|---|---|---|---|
| `rq` | 1.1.1 Full research question | textarea | §1.1.1 |
| `theory` | 1.1.2 Background theory | textarea | §1.1.2 |
| `variables` | 1.2.1 Variables | textarea | §1.2.1 |
| `method` | 1.2.2 Measurement method | textarea | §1.2.2 |
| `safety` | 1.2.3 Safety & practical considerations | textarea | §1.2.3 |
| `procedure` | 1.3.1 Step-by-step procedure | textarea | §1.3.1 |
| `img_setup` | Setup / equipment photo | image upload | After §1.3.1 |

### Data Analysis

| ID | Subsection | Type | Word doc placement |
|---|---|---|---|
| `raw_data` | 2.1.1 Raw data table | textarea | §2.1.1 |
| `img_raw_data` | Data table (if created externally) | image upload | §2.1.1 |
| `calculations` | 2.1.2 Mean and uncertainty | textarea | §2.1.2 |
| `img_calc` | Sample calculation photo | image upload | §2.1.2 |
| `proc_data_1` | 2.1.3 Processed data table 1 | textarea | §2.1.3 |
| `img_proc_data_1` | Processed data table image | image upload | §2.1.3 |
| `graph_raw` | 2.1.4 Raw data graph (terminal velocity vs surface area) | textarea | §2.1.4 |
| `img_graph_raw` | Raw graph image | image upload | §2.1.4 |
| `lin_processing` | 2.2.1 Linearisation data processing | textarea | §2.2.1 |
| `img_lin_processing` | Linearisation working image | image upload | §2.2.1 |
| `lin_table` | 2.2.2 Linearised data table | textarea | §2.2.2 |
| `img_lin_table` | Linearised data table image | image upload | §2.2.2 |
| `graph_lin` | 2.2.3 Linearised graph (terminal velocity² vs 1/surface area) | textarea | §2.2.3 |
| `img_graph_lin` | Linearised graph image | image upload | §2.2.3 |

### Conclusion

| ID | Subsection | Type | Word doc placement |
|---|---|---|---|
| `conclusion` | 3.1.1 Answer the RQ | textarea | §3.1.1–3.1.2 |
| `comparison` | 3.2.1 Compare to theory | textarea | §3.2.1 |

### Evaluation

| ID | Subsection | Type | Word doc placement |
|---|---|---|---|
| `weaknesses` | 4.1.1 Weaknesses and their impact | textarea | §4.1.1 |
| `improvements` | 4.2.1 Specific improvements | textarea | §4.2.1 |

### 4.2 Metadata fields

| Field | Type | Purpose |
|---|---|---|
| `student_name` | text | Title page, filename |
| `research_question` | text | Title page, filename `[RQ] - IA.docx` |
| `session_date` | date | Title page |

### 4.3 localStorage schema

Key: `ib-physics-ia-form`

```json
{
  "student_name": "...",
  "research_question": "...",
  "session_date": "2026-05-24",
  "rq": "...",
  "theory": "...",
  "variables": "...",
  "method": "...",
  "safety": "...",
  "procedure": "...",
  "img_setup": "data:image/png;base64,...",
  "raw_data": "...",
  "img_raw_data": "data:image/png;base64,...",
  "calculations": "...",
  "img_calc": "data:image/png;base64,...",
  "proc_data_1": "...",
  "img_proc_data_1": "data:image/png;base64,...",
  "graph_raw": "...",
  "img_graph_raw": "data:image/png;base64,...",
  "lin_processing": "...",
  "img_lin_processing": "data:image/png;base64,...",
  "lin_table": "...",
  "img_lin_table": "data:image/png;base64,...",
  "graph_lin": "...",
  "img_graph_lin": "data:image/png;base64,...",
  "conclusion": "...",
  "comparison": "...",
  "weaknesses": "...",
  "improvements": "..."
}
```

## 5. Word Document Layout

### 5.1 Formatting (standard academic)

| Property | Value |
|---|---|
| Font | Times New Roman, 12 pt |
| Line spacing | Double (2.0) |
| Margins | 1 inch (2.54 cm) all sides |
| Page size | A4 (21.0 cm × 29.7 cm) |
| Page numbers | Bottom centre, starting from page 2 |
| Headers | None on title page; running header on subsequent pages |

### 5.2 Document structure

1. **Title page** — Research question, student name, date
2. **Section 1: Research Design** (with subsections as headings)
   - 1.1.1 Research question
   - 1.1.2 Background theory
   - 1.2.1 Variables
   - 1.2.2 Measurement method
   - 1.2.3 Safety considerations
   - 1.3.1 Procedure *(plus setup image)*
3. **Section 2: Data Analysis**
   - **2.1 Raw Data Processing**
     - 2.1.1 Raw data *(plus data table image)*
     - 2.1.2 Mean and uncertainty *(plus calculation image)*
     - 2.1.3 Processed data table 1 *(plus table image)*
     - 2.1.4 Raw data graph *(plus graph image)*
   - **2.2 Linearisation**
     - 2.2.1 Linearisation data processing *(plus working image)*
     - 2.2.2 Linearised data table *(plus table image)*
     - 2.2.3 Linearised graph *(plus graph image)*
4. **Section 3: Conclusion**
   - 3.1.1 Answer the RQ
   - 3.2.1 Comparison to theory
5. **Section 4: Evaluation**
   - 4.1.1 Weaknesses
   - 4.2.1 Improvements

### 5.3 Image handling

Images are inserted at 75% of page width, centred, with a caption below. Each image is converted to base64 via the FileReader API and embedded directly in the `.docx` (no external file references).

## 6. Build Plan

| Phase | Description | Deliverable |
|---|---|---|
| 1 | Context & spec | `CONTEXT.md` (this file) |
| 2 | HTML skeleton + CSS | Sectioned layout, responsive, progress bar, input fields |
| 3 | Auto-save logic | localStorage read/write on every input event |
| 4 | Image upload | FileReader → base64, preview thumbnails, store in localStorage |
| 5 | Progress indicator | Per-section and overall completion % |
| 6 | Word generation | Docx.js document assembly from form data + images |
| 7 | Polish & testing | Edge cases, large images, empty fields, browser compatibility |

## 7. Constraints & Non-Goals

- **Not a mobile app** — designed for desktop/laptop use (the Word doc generation is resource-intensive on phones).
- **No collaborative editing** — single-user only.
- **No LaTeX rendering** — equations are written in plain text or uploaded as images.
- **No spell-check** — students should use browser-native spell-check or write in Word.
- **No submission/publishing** — the tool produces a `.docx` that the student uploads to the IB eCoursework platform. This tool does not submit on their behalf.

## 8. Example Research Question

The form is generic (works for any IB Physics IA), but the prompts and hints throughout are illustrated with the example:

> **"How does the surface area of a parachute affect the terminal velocity of a falling small mass?"**

This example is used in the hints text but never as pre-filled answer content — students always write their own.
