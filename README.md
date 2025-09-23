# GenMusic — Notebooks & Data for Music/Audioset Experiments

> A lightweight, notebook‑first workspace for scraping small datasets, running source separation, and prototyping text‑guided music/audio edits. This README explains what’s in the repo and how to run it quickly on your laptop or Google Colab.

---

## What’s inside

The repository currently consists mostly of Jupyter notebooks and a few CSV helpers. Here’s a quick tour:

- `11785_project_data_scrape.ipynb` — data collection/scraping utilities to assemble a small experimental dataset (e.g., clips & metadata) for downstream audio tasks.
- `AudioSep_Colab.ipynb` — a Colab‑friendly notebook that demonstrates music/audio **source separation** workflow (separating vocals/instruments) so you can prepare cleaner stems for training or editing.
- `Finetune_SmallExample.ipynb` — a minimal finetuning walkthrough for a small model/dataset to validate your pipeline end‑to‑end (training loop, logging, and quick evaluation).
- `TextTuneAI (1).ipynb` — a prototype for **text‑guided** music/audio edits or prompt‑driven augmentation (e.g., “make it brighter,” “remove drums,” etc.).
- `WAME Waveform Music Editor.pdf` — a short slide/guide describing a waveform‑based music editor concept you can use for UI ideas.
- `sample_generated_dataset/` — miniature example outputs used by the notebooks (e.g., separated stems or augmented clips).
- `GeneratedData_ForReference/` — additional generated samples for reference/comparison.
- `insertion_prompts.csv` & `deletion_prompts.csv` — prompt lists to drive text‑guided edits or data augmentation (insert/remove style/instrumental elements).
- `instruments List.csv` — canonical instrument labels useful for conditioning, tagging, or evaluation.

> Note: The repo does not include a Python package or CLI; execution happens inside the notebooks.

---

## Quick start

### Option A — Run locally (conda + Jupyter)

1. **Install Conda (miniforge/miniconda)** if you don’t have it.
2. Create an environment (Python 3.10+ recommended):
   ```bash
   conda create -n genmusic python=3.10 -y
   conda activate genmusic
   ```
3. **Install common audio/ML packages** used by the notebooks:
   ```bash
   pip install jupyter notebook numpy pandas matplotlib scipy librosa soundfile torchaudio torch torchvision --index-url https://download.pytorch.org/whl/cpu
   pip install tqdm requests
   ```
   - If you have a CUDA GPU, replace the PyTorch install line with the CUDA‑specific command from pytorch.org.
4. **Launch Jupyter** and open any notebook:
   ```bash
   jupyter notebook
   ```
5. Start with `Finetune_SmallExample.ipynb` to verify your environment. Then explore `AudioSep_Colab.ipynb` and `TextTuneAI (1).ipynb`.

### Option B — Google Colab (zero‑setup)

Most notebooks work out‑of‑the‑box on Colab. Open a notebook in GitHub → click **“Open in Colab”** (or paste the GitHub URL into Colab). The notebook will install its own dependencies in the first cells when provided.

---

## Typical workflows

### 1) Build a tiny dataset
- Open **`11785_project_data_scrape.ipynb`** and run the cells to download/curate a few minutes of audio with labels/metadata for experiments.
- Save outputs under `sample_generated_dataset/` for consistency.

### 2) Separate stems (prep data)
- Use **`AudioSep_Colab.ipynb`** to run source separation on your audio (vocals, drums, bass, other). Export WAV/FLAC stems for cleaner supervision or evaluation.

### 3) Text‑guided edits / augmentation
- Load **`TextTuneAI (1).ipynb`**.
- Use `insertion_prompts.csv` / `deletion_prompts.csv` as prompt libraries to systematically add/remove sonic attributes in your data or generate edited variants for training robustness.

### 4) Quick finetune
- In **`Finetune_SmallExample.ipynb`**, point the dataset path to `sample_generated_dataset` (or your own), then run the training loop to validate the pipeline. Aim for a short run (5–10 minutes) to confirm everything works.

---

## Data & file conventions

- **Audio format:** 44.1k/48kHz WAV is preferred for lossless processing. Keep stereo unless you know you need mono.
- **Paths:** Use relative paths inside notebooks (e.g., `./sample_generated_dataset/clip001.wav`) so Colab/local runs behave the same.
- **Metadata:** Store CSV/JSON sidecars alongside audio files (`clip_id, instrument, prompt, split`).

---

## Repro tips

- **Determinism:** set `PYTHONHASHSEED=0`, `torch.manual_seed(42)`, and NumPy seed for repeatability.
- **Small first:** try 1–5 short files to validate the pipeline before scaling up.
- **GPU vs CPU:** separation and finetuning are faster on GPU; Colab T4 is usually enough for demos.

---

## Project roadmap (suggested)

- [ ] Convert notebooks into reusable Python modules (e.g., `genmusic/`).
- [ ] Add a simple CLI: `genmusic separate`, `genmusic finetune`, `genmusic edit --prompt "...“`.
- [ ] Publish a minimal model card and example audio before/after.
- [ ] Add tests and CI to smoke‑test notebook cells.
- [ ] Pin exact package versions with `requirements.txt` and `environment.yml`.

---

## Requirements (baseline)

- Python 3.10+
- Jupyter Notebook / JupyterLab
- PyTorch + Torchaudio (CPU or CUDA), Librosa, NumPy, SciPy, Matplotlib, SoundFile, TQDM, Requests

> Individual notebooks may install extra libs inline (e.g., separation backends). Follow the first “setup” cell in each notebook.

---

## FAQ

**Q: Do I need a GPU?**  
No, but finetuning and separation will be much faster with one. Colab GPU is fine for demos.

**Q: Where should I put my own audio?**  
Create a folder like `data/` or reuse `sample_generated_dataset/` and update the notebook cell that defines the input path.

**Q: Can I export examples to share?**  
Yes—write files under `GeneratedData_ForReference/` so they don’t get mixed with training inputs.

---

## License

No explicit license is present in the repo. If you plan to publish code or trained assets, consider adding a LICENSE (e.g., MIT/Apache‑2.0 for code; Creative Commons/AudioSet licenses for datasets you didn’t author).

---

## Acknowledgements

This repo appears to be an experimental sandbox for course/individual research. It reuses common audio ML practices: small‑scale scraping, source separation, prompt lists for text‑guided edits, and a minimal finetuning loop. Props to the original notebooks’ authors and upstream libraries used within them.

