# Participant Report

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

**Generates a participant-friendly HTML report with interactive viewers for a T1w image, FreeSurfer surfaces and parcellation, white matter tracts and BOLD thumbnails.**

Builds an HTML page section by section for whichever inputs are given: a Papaya viewer of the T1w, FreeSurfer T1, aparc+aseg and pial surfaces, a tract viewer for the wmc tracts and surfaces, and thumbnails of the BOLD series. Use it to share an easy-to-read summary of a participant's data. All inputs are optional; the FreeSurfer section needs a FreeSurfer license.

Run it on brainlife.io: [https://doi.org/10.25663/brainlife.app.601](https://doi.org/10.25663/brainlife.app.601)

| Input | Datatype | Required | Description |
|---|---|---|---|
| `fs` | `neuro/freesurfer` | no | FreeSurfer output; T1, aparc+aseg and pial surfaces are shown in the report. |
| `wmc` | `neuro/wmc` | no | White matter classification with tract and surface files, shown in an interactive tract viewer. |
| `task` | `neuro/func/task` | no | fMRI time series; thumbnails are generated for the report. |
| `anat` | `neuro/anat/t1w` | no | T1-weighted image shown in an interactive volume viewer. |

| Output | Datatype | Description |
|---|---|---|
| `output` | `report/html` | HTML report (index.html) with embedded viewers and images. |

Runtime: container `brainlife/dipy:1.4.0`; 4 CPUs; 10 GB memory; 0.25 h walltime; needs `FREESURFER_LICENSE`.

<!-- brainlife-app:end -->

This brainlife App will generate report HTML for study participants from various datatypes.


