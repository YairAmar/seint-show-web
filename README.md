# seint-show-web

Interactive companion to "Probing Layer-Wise Robustness and Sensitivity of
Speech Enhancement Models Under Noise and Reverberation" by Yair Amar, Amir Ivry
and Israel Cohen, Andrew and Erna Viterbi Faculty of Electrical and Computer
Engineering, Technion, Israel Institute of Technology.

Live demo: https://yairamar.github.io/seint-show-web/

Preprint: https://arxiv.org/abs/2512.00482

An earlier version of this demo was shown at the ICASSP 2026 Show & Tell session
in Barcelona (submission no. 553).

## What it shows

Speech enhancement models improve quickly, but how input degradation moves their
internal representations is much less examined. The paper probes three models,
MUSE, MP-SENet and Demucs, across controlled levels of SNR and reverberation
(quantified by C50). It measures layer-wise similarity to clean references with
Centered Kernel Alignment (CKA), then summarises each layer with a linear fit
against degradation level, where the intercept measures robustness and the slope
measures sensitivity.

All three models turn out to be sharply non-uniform across depth, but they
organise that non-uniformity differently. MUSE and MP-SENet grow more sensitive
with depth, and the sharpest transitions fall at MUSE's skip-connection
junctions, where encoder information is reintegrated. Demucs inverts the trend. A
randomly initialised model is close to flat, with slopes one to two orders of
magnitude smaller, and the profile only forms during fine-tuning. That points to
the enhancement objective as the cause rather than any particular architecture.

This demo lets you drive the experiment yourself. Pick a clean utterance, add a
noise clip, move the SNR slider, and watch the per-layer CKA respond: the encoder
stays close to the clean reference while deeper layers pull away.

## What this repository contains

Build output only. This is the compiled single-page app, served by GitHub Pages.
It needs no server. The application source, the backend and the research code
are not part of this repository.

`static-api/` holds the backend's responses for the six curated voices and the
five shipped noises, at every SNR from -10 to 30 dB, for all three models. The
page reads those files instead of calling an API. It is produced from the
project's `backend/` and `frontend/` directories with:

```
cd backend
python scripts/export_static_api.py ../frontend/public/static-api
cd ../frontend
VITE_STATIC_API=true npx vite build --base=/seint-show-web/ --sourcemap false
```

## Notes

- Most CKA values are representative values shaped like the paper's layer
  profiles, from the backend's mock mode, the same mode the earlier hosted API
  ran. That is enough to show the effect, but it is not measured output for
  your clip.
- The enhanced audio comes from an oracle filter that uses the clean reference,
  so it can sound better than a real model would.
- Recording your own voice or noise needs the backend and is not available here.
- When a compute backend runs on your own machine at `http://127.0.0.1:8000/api`,
  the page still uses it for the noise sweeps.

## License

MIT.
