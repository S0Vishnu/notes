

## Linear hide Blur effect

-webkit-backdrop-filter: blur(6px);
backdrop-filter: blur(6px);
width: 100%;
height: 100%;
position: absolute;
inset: 0;
-webkit-mask-image: linear-gradient(#000, #000 30%, #0000);
mask-image: linear-gradient(#000, #000 30%, #0000);

---
# invert bg

  isolation: isolate;
  mix-blend-mode: difference;
  or
  mix-blend-mode: exclusion;
