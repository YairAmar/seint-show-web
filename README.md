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
It calls a separate API for inference. The application source, the backend and
the research code are not part of this repository.

It is produced from the project's `frontend/` directory with:

```
VITE_API_BASE=<api-origin>/api npx vite build --base=/seint-show-web/ --sourcemap false
```

## Notes

- The heavy computation is designed to run on the presenter's own machine. When
  no local compute is reachable, the hosted API returns representative values
  shaped like the paper's layer profiles. That is enough to show the effect, but
  it is not measured output for your specific clip.
- Microphone capture is optional. Recorded clips are held only for your own
  session, are not stored on the server, and are not visible to anyone else.
- The hosted API is a free-tier deployment with a scheduled shutdown. If the page
  loads but reports the backend as unreachable, that is the reason.

## License

MIT.
