# mimo-channel-estimation

**SRS-pilot-based LS channel estimation for a 4 × 64 MIMO–OFDM uplink, with downlink spectral-efficiency analysis under estimated (rather than perfect) CSI.**

This project drops the perfect-CSI assumption used in `mimo-spatial-filtering`: the base station has to *estimate* the channel from received Sounding Reference Signal (SRS) pilots. The work implements LS estimation, analyses the channel's delay structure (CIR, PDP), and quantifies how the resulting estimation error propagates into SVD-precoded downlink performance.

## What this project investigates

1. **How LS channel estimation actually works on a real-shaped MIMO–OFDM link** — uplink reception of SRS, projection onto the known pilot, recovery of `Ĥ` from the noisy uplink under TDD reciprocity.
2. **The channel's temporal structure**, made visible from the estimate — channel impulse response via IDFT along subcarriers, power delay profile averaged across antenna pairs.
3. **The end-to-end cost of imperfect CSI** — the same SVD-precoded downlink from the previous study, but now evaluated on the *true* channel using a precoder built from `Ĥ`. Spectral-efficiency CDFs across uplink SNR and rank.

## Method (short)

- **Uplink:** UE transmits known SRS pilot; BS observes `Y = H · x_SRS + n`. LS estimate `Ĥ = Y · x_SRS^† / ‖x_SRS‖²`.
- **Channel structure:** CIR `z(τ) = IDFT_K{H}`, PDP `= |z(τ)|²` averaged over antenna pairs.
- **TDD reciprocity:** estimated uplink channel reused as the downlink channel estimate for precoder construction.
- **Downlink eval:** build SVD precoder on `Ĥ`, score spectral efficiency on `H` (the honest "deploy on the true channel" metric, not the self-evaluation pitfall).
- Sweep across uplink SNR and transmission rank; report median spectral-efficiency curves and CDFs.

## What's in here

| File | Purpose |
|---|---|
| `notebooks/channel_estimation.ipynb` | Complete notebook with code, derivations, and figures |
| `report.pdf` | Written report (PDF — distribution-controlled) |
| `LICENSE` | MIT |

## Data

Course-provided MIMO channel `.mat` files plus the SRS pilot signal (`srs_sig.mat`). **Not included in this repository** (course-restricted). Place them in the directory referenced by `FOLDER_PATH` at the top of the notebook.

## Running it

```bash
pip install numpy scipy matplotlib tqdm ModulationPy
```

Open the notebook and run cells top-to-bottom. Pure NumPy/SciPy — no GPU required.

## Context

Coursework for the **MIMO Wireless Communications** course at the **Skolkovo Institute of Science and Technology (Skoltech)**, M.Sc. in IoT and Wireless Technologies. Sits between [`mimo-spatial-filtering`](https://github.com/MuhammadHanzalaIqbal/mimo-spatial-filtering) (perfect CSI) and [`mimo-channel-denoising`](https://github.com/MuhammadHanzalaIqbal/mimo-channel-denoising) (improving the noisy LS estimate).

## License

MIT — see [`LICENSE`](LICENSE).

---

*Author: Muhammad Hanzala Iqbal.*

## Related projects

- [`mimo-spatial-filtering`](https://github.com/MuhammadHanzalaIqbal/mimo-spatial-filtering) — perfect-CSI baseline study (SVD precoding, beam-domain analysis).
- [`mimo-channel-denoising`](https://github.com/MuhammadHanzalaIqbal/mimo-channel-denoising) — improving the noisy LS estimate from this project with adaptive denoising.
- [`nanoGPT-rf-rope-ablation`](https://github.com/MuhammadHanzalaIqbal/nanoGPT-rf-rope-ablation) — small-transformer architectural ablation (RoPE vs learned PE).
