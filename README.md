A real-time generative visual system built in TouchDesigner 2025.32820.

## Project Structure

```
project1/
├── scene_3d        — 3D cathedral geometry, camera, lights, pipe SOPs
├── final_comp      — full compositing chain
│   ├── fog_glsl        — procedural mist shader
│   ├── riser_glsl      — glyph rise/evaporate shader
│   ├── crt_shader      — CRT post-processing
│   ├── ascii_glsl      — ASCII conversion (in progress)
│   └── fog_late_comp   — final additive composite
└── cathedral_builder   — Python DAT setup scripts
```

## Composite Chain

```
render_in → beam_comp → fog_comp → glyph_comp
→ glow_blur → glow_level → crt_shader
→ fog_late_comp → ascii_comp → txt_comp → out1
```

## What controls what
- **`cam_frameexec`** — locks all camera parameters AND riser glyph properties every frame. Editing camera or riser params directly in the UI has no effect; they are overridden on the next cook. **Always edit `cam_frameexec`.**
- **`fog_late_comp`** — the final composite. Its operand must stay `add`. Changing it to `over` collapses the fog and breaks the scene.


## Locked Values — Do Not Change

These are not defaults; they are load-bearing for the visual.

| Parameter | Value | Why |
|---|---|---|
| Camera `tz` | `9.0` | Depth / perspective sweet spot |
| Camera `fov` | `85.0` | Matched to pipe layout |
| Camera `ty` | `~0.27` | Fog V-shape alignment |
| Camera `rx` | `-3.0` | Pipes appear vertical/parallel |
| `fog_late_comp` operand | `add` | Fog glow — `over` breaks it |
| Pipe `ty` formula | `0.8 + i * 0.28` | Cathedral corridor perspective |
| Pipe `tz` spacing | `i * -3.0` | Corridor depth |


## Working with Pipes

- `ty` is the **anchor point**, not a visual offset. Changing it shifts the whole pipe including its base and breaks corridor alignment.
- The corridor perspective comes entirely from `tz` spacing — adjust depth there, not `ty`.
- **Never use `-=` on pipe `ty` in a loop** — it accumulates across multiple runs in the same session.
- Do not change `box1 sizey` — it extends the pipe equally up and down from center. Making pipes longer at the bottom also requires offsetting `ty`, which breaks the corridor.


## Influences

https://www.deviantart.com/julian-faylona

## License

Do whatever you want with it.
