<h1 align="center">Participant Report</h1>

<p align="center">
  A participant-friendly HTML report with interactive viewers for a T1-weighted image, FreeSurfer output, white matter tracts and BOLD thumbnails.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.601"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.601-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

## 🧠 At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Generates a participant-friendly HTML report with interactive viewers for a T1w image, FreeSurfer surfaces and parcellation, white matter tracts and BOLD thumbnails.**

Builds an HTML page section by section for whichever inputs are given: a Papaya viewer of the T1w, FreeSurfer T1, aparc+aseg and pial surfaces, a tract viewer for the wmc tracts and surfaces, and thumbnails of the BOLD series. Use it to share an easy-to-read summary of a participant's data. All inputs are optional; the FreeSurfer section needs a FreeSurfer license.

| | |
|---|---|
| 📥 **Inputs** | `neuro/freesurfer` (optional) · `neuro/wmc` (optional) · `neuro/func/task` (optional) · `neuro/anat/t1w` (optional) |
| 📤 **Outputs** | `report/html` |
| 🐳 **Container** | `brainlife/dipy:1.4.0` |
| ⚙️ **Resources** | 4 CPUs · 10 GB memory · 0.25 h walltime |
| 🔑 **Requires** | `FREESURFER_LICENSE` |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.601](https://doi.org/10.25663/brainlife.app.601) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `fs` | `neuro/freesurfer` | no | FreeSurfer output; T1, aparc+aseg and pial surfaces are shown in the report. |
| `wmc` | `neuro/wmc` | no | White matter classification with tract and surface files, shown in an interactive tract viewer. |
| `task` | `neuro/func/task` | no | fMRI time series; thumbnails are generated for the report. |
| `anat` | `neuro/anat/t1w` | no | T1-weighted image shown in an interactive volume viewer. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `report/html` | HTML report (index.html) with embedded viewers and images. |

<!-- brainlife-app:end -->

## 🚀 Running the App

### ☁️ On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.601](https://doi.org/10.25663/brainlife.app.601), choose any of the optional inputs from your project for one participant and press **Execute**. The result is a `report/html` dataset that you can open in the browser and share with the study participant.

The report starts with a header naming the subject (and session, when known) and adds one section for each input you give:

- **T1-weighted image** (`neuro/anat/t1w`): an interactive [Papaya](https://github.com/rii-mango/Papaya) volume viewer.
- **FreeSurfer output** (`neuro/freesurfer`): the FreeSurfer `T1` and `aparc+aseg` volumes and the left and right pial surfaces, converted to NIfTI and GIFTI with FreeSurfer 7.1.1.
- **White matter classification** (`neuro/wmc`): an interactive tract viewer built from the WMC tracts (and surfaces, when present).
- **fMRI time series** (`neuro/func/task`): thumbnails of every tenth volume, showing its deviation from the mean BOLD image in sagittal, coronal and axial views.

### 💻 Locally

You need [Singularity/Apptainer](https://apptainer.org/) and [`jq`](https://jqlang.github.io/jq/). Python (DIPY 1.4.0 container) and FreeSurfer 7.1.1 (`brainlife/freesurfer-mini`) run inside containers. A [FreeSurfer license](https://surfer.nmr.mgh.harvard.edu/registration.html) is needed only for the FreeSurfer section.

1. Clone this repository.
2. Create `config.json` with the paths to the inputs you have (leave out the ones you don't); `config.json.template` shows a full example. The header reads the subject (and session) from the first entry of `_inputs`:

   ```json
   {
       "t1": "/input/t1/t1.nii.gz",
       "fs": "/input/freesurfer/output",
       "wmctracts": "/input/wmc/tracts",
       "wmcsurfaces": "/input/wmc/surfaces",
       "bold": "/input/func/bold.nii.gz",
       "_inputs": [
           { "meta": { "subject": "123" } }
       ]
   }
   ```

3. Export your license (if you give a FreeSurfer input) and run the App:

   ```bash
   export FREESURFER_LICENSE="$(cat ~/freesurfer/license.txt)"
   ./main
   ```

## 📦 Outputs

```text
output/html/
├── index.html    the report
├── anat/         T1-weighted image (t1.nii.gz)
├── fs/           T1.nii.gz, aparc+aseg.nii.gz, lh/rh.pial.gii
├── tracts/       WMC tracts used by the tract viewer
├── tractview/    tract viewer page and assets
├── bold/         BOLD thumbnails (thumb<N>.png)
├── papaya/       Papaya volume viewer
└── fsview/       FreeSurfer viewer
```

Only the sections for the inputs you provide are created.

## 🩺 Troubleshooting

- **`Please set FREESURFER_LICENSE in .bashrc`**: a FreeSurfer input was given but no license is available. Set `FREESURFER_LICENSE` (on the brainlife.io compute resource, or in your shell when running locally).

## 📚 Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Fischl B. FreeSurfer. *NeuroImage* 62(2):774–781 (2012). [doi:10.1016/j.neuroimage.2012.01.021](https://doi.org/10.1016/j.neuroimage.2012.01.021)
- Participant Report. brainlife.io App. [doi:10.25663/brainlife.app.601](https://doi.org/10.25663/brainlife.app.601)
- Garyfallidis et al. Dipy, a library for the analysis of diffusion MRI data. *Frontiers in Neuroinformatics* 8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## 👥 Authors

- Soichi Hayashi

## 💰 Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## ⚖️ License

[MIT](LICENSE). FreeSurfer itself is distributed under its own [license](https://surfer.nmr.mgh.harvard.edu/fswiki/FreeSurferSoftwareLicense).
