# Figure 1: forward–inverse agreement

`figure-1.png` renders the complete Figure 1 diagram from page 2 of the current compiled
paper, `iclr2027_conference.pdf`. Its source is `figures/forward-inverse.tex`. It
includes the coding agent, scene specification, renderer, forward and inverse programs,
connecting arrows, and verifier. The paper figure is unchanged; the page header, line
numbers, caption, and surrounding manuscript text are outside the rendering crop. The
README provides the explanation in searchable text beneath the image.

The PNG is 1684 × 692 pixels, rendered at 300 DPI with Poppler:

```bash
pdftoppm -f 2 -l 2 -r 300 -x 433 -y 325 -W 1684 -H 692 \
  -png -singlefile iclr2027_conference.pdf figure-1
```

Run this command beside the compiled paper to reproduce the image. The paper source/PDF
is intentionally not bundled with the execution release. The diagram is a reader
illustration, not an agent input or a runnable world bundle.

SHA-256: `10f0d245cc91f03d73f837a794351c509e345ea9337dfe348b95d77ccb088fae`.
