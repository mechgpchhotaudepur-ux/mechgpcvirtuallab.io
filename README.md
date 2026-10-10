# Virtual Lab Console - Mechanical Engineering, GP Chhotaudepur

A static website (no server) that runs on **GitHub Pages**. Students:

1. sign in with their GTU enrolment number (the semester is filled in from it),
2. see only the virtual labs mapped to their semester's courses (labs L1-L16 from the Virtual Laboratory Training Document),
3. open each experiment on vlabs.ac.in, with a prepare checklist and a timer,
4. write observations, calculations, result and a self-evaluation in a **record sheet**,
5. save, print (or save as PDF) and copy the record sheet; optionally it is sent to a department Google Sheet.

Faculty get a **session log** form (the dated record for SAR 2.2.3 B) that can be copied into Excel, downloaded as CSV, or sent to the same Google Sheet.

## Files

| File | What it is | Edit it when |
|---|---|---|
| `index.html` | The whole console (page, styles, logic) | Changing the layout |
| `data/labs.js` | 16 labs (156 experiments) plus the 3 SAR experiments, all with links, course-to-lab mapping, gaps, free tools | A lab or experiment changes, or a course mapping is confirmed |
| `config.js` | Institute name, contact email, Google Sheet link, enrolment-number rule | Once, at setup |
| `apps-script/Code.gs` | Google Apps Script that writes records into a Google Sheet | Only for setup |
| `scripts/check-links.mjs` + `.github/workflows/link-check.yml` | Checks every link each Monday and writes `LINK-REPORT.md` | Never |

## Put it on GitHub Pages (about 5 minutes)

1. On github.com, sign in to the department account and click **New repository**. Name it `virtual-lab` (any name works). Public. Create.
2. Click **uploading an existing file**, drag in **all** files and folders from this folder (including `.github` and `.nojekyll`; on Windows, show hidden files first). Commit.
3. **Settings > Pages**: Source = *Deploy from a branch*, Branch = `main`, folder `/ (root)`. Save.
4. After a minute the site is at `https://<account>.github.io/virtual-lab/`. Share that link with students (WhatsApp / notice) and keep a screenshot as proof the link was shared.
5. **Settings > Actions > General > Workflow permissions**: choose *Read and write*, so the weekly link check can save its report. Run it once from **Actions > Weekly link check > Run workflow**.

To link it from the department website, add a button pointing to the Pages URL.

## Collect records in a Google Sheet (optional, recommended)

Without this step, records stay in each student's browser and students print or copy them for you.

1. Create a Google Sheet with the department account.
2. **Extensions > Apps Script**, paste `apps-script/Code.gs`, save.
3. **Deploy > New deployment > Web app**, Execute as *Me*, Who has access *Anyone*. Authorise.
4. Copy the URL that ends in `/exec`, paste it into `config.js` as `sheetEndpoint: "https://script.google.com/macros/s/.../exec"`, commit.

Each saved record sheet then appears as a row on the **Student records** tab, and each faculty entry on the **Session log** tab, with the date and time. Add rubric marks in the last column.

## Editing labs and courses

Open `data/labs.js` on GitHub, click the pencil, edit, commit. Examples:

- Add an experiment to a lab: add a line `["Title", "exp/slug/"],` in that lab's `exps` list.
- Confirm or change a course mapping: edit the `courses` list at the bottom (`sem`, `name`, `labs`, `support: true` for a supporting lab, `note`).

Experiment codes (for example `L9·E04` = Fluid Mechanics NITK, Friction in pipes) follow the order in the list, so add new experiments at the end of a lab to keep old codes stable.

## Notes

- Enrolment numbers are checked against the pattern `YY622[0|8]319NNN` (regular / C2D, Mechanical, institute 622). No student list is published on the site.
- Data on this site comes from vlabs.ac.in lab pages read on 10 Oct 2026. L1 now lists 10 experiments and L6 lists 8 (the data sheet of 05 Oct 2026 counted 9 and 7).
- The browser keeps drafts, so a student who closes the experiment sheet can continue later on the same device.
