# aiML

Exploratory work — notebooks written to learn machine learning concepts by
building them from scratch rather than reading about them.

Nothing here is a library or a product. It's a sandbox.

## Notebooks

- **[`research-notebooks/01-chain-of-thought-prompting.ipynb`](research-notebooks/01-chain-of-thought-prompting.ipynb)** — an experimental replication lab for Wei et al. (2022): run direct, chain-of-thought, and ablation prompts on a deterministic GSM8K sample with an open Qwen model; measure accuracy, uncertainty, tokens, latency, GPU memory, and errors. Designed for a free Colab T4; no API key required. See the [research-notebooks index](research-notebooks/README.md).

  [![Open replication lab 01 in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/webpug/aiML/blob/main/research-notebooks/01-chain-of-thought-prompting.ipynb)
- **`math_of_ml_screensaver.ipynb`** — the math behind ML, worked up from dot
  products and gradient descent to a tiny MLP trained *by hand* (no autograd).
  The learned scalar field becomes a divergence-free flow, and particles running
  through it produce a generative screensaver. A final section scales the same
  idea to a GPU CPPN in PyTorch.
- **`epistemic_vs_aleatoric.ipynb`** — the two kinds of uncertainty in a
  prediction: what the model doesn't know yet, versus what nothing could know.
- **`ml_concepts_screensavers.ipynb`** — builds on the screensaver notebook:
  diffusion models, state-space models and transformers, each explained from
  its core equation, built as a working toy, and rendered as a 15-minute
  seamlessly looping screensaver. Renders at 3440 × 1440 for a Dell 34" 21:9
  curved ultrawide by default; `SAVER_DISPLAY=portrait` renders 480 × 1440 for
  the 1:3 display of the first notebook instead.

The `.mp4` and `.png` files are rendered output from the screensaver notebooks.
The 15-minute `*_loop.mp4` renders are git-ignored (1.1–1.3 GB each at
3440 × 1440, far over GitHub's 100 MB file limit), so they exist only where the notebook was run;
the committed `*_still.png` posters show one frame of each. Re-run the concepts
notebook to regenerate them.

## Running

```bash
python -m venv .venv && source .venv/bin/activate
pip install numpy matplotlib jupyterlab
jupyter lab                                   # or: jupyter lab <notebook>.ipynb
```

The first research-paper lab installs its pinned-range Hugging Face dependencies in Colab and requires an NVIDIA GPU for the default 3B-model experiment.

The GPU section of `math_of_ml_screensaver.ipynb` additionally needs `torch` and
an NVIDIA GPU. Its MP4 export uses `ffmpeg` if it's on PATH, and falls back to GIF
if not.

`ml_concepts_screensavers.ipynb` needs `torch`, a CUDA GPU and `ffmpeg` on PATH;
there is no GIF fallback because frames are streamed straight into `ffmpeg`. The
notebook is committed with its outputs, so it reads fine without re-running.
A full run renders three 15-minute 3440 × 1440 loops, which took about 13, 17
and 17 minutes (diffusion, SSM, transformer) on a GTX 1070, encoding on its
NVENC block. For
quick iteration, shorten every loop:

```bash
LOOP_SECONDS=30 jupyter lab ml_concepts_screensavers.ipynb
```

### Opening the browser from WSL

Under WSL, Jupyter can't find a browser by default ("Couldn't find a suitable web
browser!"). Point it at the Windows default browser in
`~/.jupyter/jupyter_server_config.py`:

```python
c.ServerApp.browser = '/mnt/c/Windows/explorer.exe %s'
c.ServerApp.use_redirect_file = False   # pass the http:// URL; Windows can't open the file:// redirect page in WSL
```

Or skip it and paste the `http://localhost:8888/lab?token=...` URL from the
terminal into a Windows browser.
