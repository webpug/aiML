# aiML

An educational, single-notebook tour of the math behind machine learning that
builds up to a generative artifact: a vertical neural-flow screensaver designed
for a 30" × 10" portrait display (1:3 aspect).

The notebook starts from dot products and gradient descent, trains a tiny MLP
*by hand* (no PyTorch, no autograd) to fit a scalar potential, turns the
learned potential into a divergence-free flow field, and runs particles through
it to produce the screensaver. A second half scales the same idea up to a
GPU-accelerated, time-conditioned CPPN with mixed activations and per-particle
color.

## Layout

- `math_of_ml_screensaver.ipynb` — the whole project. Sections 1–7 are the
  NumPy/CPU build; section 8 is the PyTorch/GPU CPPN extension.
- `neural_flow_screensaver.mp4` / `neural_flow_still.png` — CPU artifacts.
- `neural_flow_gpu.mp4` / `neural_flow_gpu_still.png` — GPU CPPN artifacts.
- `ml_concepts_screensavers.ipynb` — second notebook, builds on the first:
  diffusion models, SSMs and transformers, each as a 15-minute looping
  screensaver (`diffusion_loop.mp4`, `ssm_loop.mp4`, `transformer_loop.mp4`,
  git-ignored; `*_still.png` posters are committed). Frames stream straight
  into ffmpeg; `LOOP_SECONDS` env var shortens the loops for iteration.
- `.venv/` — local virtualenv (not committed).

## Running it

```bash
source .venv/bin/activate
jupyter lab math_of_ml_screensaver.ipynb       # or ml_concepts_screensavers.ipynb
```

Hard deps: `numpy`, `matplotlib`. GPU section adds `torch`. MP4 export needs
`ffmpeg` on PATH; without it the notebook falls back to GIF.

The concepts notebook hard-requires `torch`, CUDA and `ffmpeg` (it asserts on
`ffmpeg`; no GIF fallback). Full render ≈ 6 / 26 / 10 min for diffusion / SSM /
transformer on the shared GTX 1070; use `LOOP_SECONDS=30` while iterating.

WSL has no Linux browser. `~/.jupyter/jupyter_server_config.py` (user config,
not in the repo) sets `c.ServerApp.browser = '/mnt/c/Windows/explorer.exe %s'`
and `use_redirect_file = False` so Jupyter opens the Windows default browser
with the http URL.

## Design notes worth knowing before editing

- **No autograd in the CPU half — on purpose.** Backprop through the one-tanh
  MLP is written out explicitly so the math stays visible. Don't refactor it to
  call `torch.autograd`; that defeats the pedagogical point.
- **Stream-function trick.** Flow is the *perpendicular* gradient of the
  learned scalar field, which makes it divergence-free by construction. This
  is why particles swirl rather than collapse into sinks.
- **Trails come from an accumulating image buffer**, not scatter markers. The
  buffer fades each frame (`fade≈0.95`) and particles splat intensity into it.
  Scatter plots cannot reproduce the dense, glowing look.
- **Canvas is 1:3 portrait** throughout (`DOM_W=1.6, DOM_H=4.8` on the CPU
  side; `GPU_DOM_W=1.5, GPU_DOM_H=4.5` on the GPU side). Changing aspect
  requires re-tuning particle spawn distributions and drift.
- **GPU stream-function uses `autograd.grad` once per frame.** That backward
  pass is the bottleneck — ~25 ms for 90k particles on a GTX 1070.
- **The concepts notebook renders seamless loops by crossfade.** `render_loop`
  simulates past the end and blends the tail into the head; every driver must
  be a periodic function of `phase(t, k)` or the seam shows.
- **Transformer screensaver: β must beat ln N.** With thousands of tokens the
  softmax's far-token background mass is ~N·e^-β; the separation head leaks
  into a push away from the global centroid unless its β is sharp (24).
- **The GPU is shared with the Windows host.** `nvidia-smi` has shown 6 GB VRAM
  and ~85 % utilisation taken by host processes invisible to WSL; renders can
  be 3–5× slower than the GPU alone would suggest. Check before trusting timings.

## Conventions

- One notebook, top-to-bottom narrative. New ideas go in new sections, not in
  separate files, unless they grow past ~50 lines of code.
- Plots use the dark theme set in cell 1 (`#0b0b14` background, magma/embers
  palettes). Keep it dark — the screensaver context is dark.
- Cell comments are short and explain *why*, not *what*. The prose cells are
  where explanation lives.
