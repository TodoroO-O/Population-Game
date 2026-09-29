# Predators, Prey & Grass

A browser simulation of the classic **Lotka–Volterra predator–prey equations**, shown as a live game. Red predators hunt blue prey across a circular field, and blue prey graze patches of grass that regrow over time. None of the population curves are scripted. The boom-and-bust cycles come only from the individual rules each dot follows.

![Screenshot of the simulation](assets/screenshot.png)

## Play it

**▶ Play online:** `https://<your-username>.github.io/<repo-name>/`

Or run it on your own computer. It's a single HTML file with no install, build step or dependencies:

1. Click the green **Code** button on this page, then **Download ZIP**, and unzip it. You can also `git clone` the repo.
2. Double-click `index.html`. It opens in your default browser (Chrome, Firefox, Safari or Edge).

It works offline too. Without internet the page uses your system fonts instead of the Google Fonts, and everything else is the same.

## The sprites

| Sprite | What it is |
|:---:|---|
| <img src="assets/predator.svg" width="64" alt="Predator"> | **Predator.** The ring around it is its starvation timer, and it drains as the predator goes without food. The small dots inside count the prey eaten toward its next split. |
| <img src="assets/predator-starving.svg" width="64" alt="Starving predator"> | **Starving predator.** The ring turns amber and the body fades when less than 30% of the time is left. |
| <img src="assets/prey.svg" width="64" alt="Prey"> | **Prey.** Runs from predators and seeks out fresh grass. |
| <img src="assets/grass-fresh.svg" width="64" alt="Fresh grass"> | **Fresh grass.** Prey can eat it. |
| <img src="assets/grass-grazed.svg" width="64" alt="Grazed grass"> | **Grazed grass.** Just eaten and can't be eaten again until it regrows. |

Grazed patches fade from beige back to green over the regrowth time (5 s by default):

<img src="assets/grass-regrowth.svg" width="560" alt="Grass regrowing from beige to green over 5 seconds">

## Rules

### Prey (blue)
- Eating a fresh grass patch splits the prey into two. The patch turns beige and starts regrowing.
- The prey reacts to the nearest predator in three zones:
  - **Panic (within 70 px):** it ignores grass and runs straight away at full speed. It won't eat even if it runs over a patch.
  - **Alert (within about 115 px):** it mostly runs away, but it can drift toward grass and eat along the way.
  - **Calm (no predator nearby):** it heads for the nearest fresh grass, or wanders if there's none.
- It runs in jittery, slightly random directions, so predators can't predict its path.

### Predators (red)
- Chase the nearest prey in sight and are **25% faster** than prey at full speed.
- **Catching a prey** kills it and refills the predator's starvation timer (6 s by default).
- **Eating 5 prey** splits the predator into two.
- **Running out of time** kills the predator by starvation.
- **Loners:** predators keep away from each other (90 px of personal space). A predator close to starving will put up with company to reach food.

### The field
- A round arena with a soft wall. Animals that reach the rim slide along it, so there are no corners to trap prey in.
- Population caps of 320 prey and 160 predators act as a carrying capacity.

## Default parameters

The field is 860 × 860 units with an arena radius of 420, and 1 unit is about 1 screen pixel at full size.

| Parameter | Default | Slider range |
|---|---|---|
| Prey speed: fleeing / to grass / wandering | 58 / 40.6 / 26.1 units/s | fixed |
| Predator speed edge over prey | +25% (72.5 units/s) | 0–50% |
| Starvation time (predator ring) | 6 s | 2–12 s |
| Prey eaten to split | 5 | 1–10 |
| Prey panic radius | 70 px | 20–150 px |
| Predator personal space | 90 px | 0–180 px |
| Grass regrowth | 5 s | 1–15 s |
| Starting prey / predators | 40 / 6 | 5–150 / 1–30 |

## The math behind it

The classic Lotka–Volterra model describes two populations, prey *x* and predators *y*:

```
dx/dt = αx − βxy     (prey)
dy/dt = δxy − γy     (predators)
```

Each term maps to a rule in the game:

| Term | In the equation | In the game |
|---|---|---|
| α | prey growth | prey split after eating grass |
| β | predation | predators catch prey on contact |
| δ | predator growth from food | every 5 catches split a predator |
| γ | predator death | predators starve if they miss their deadline |

The side panel plots both populations over time. You can drag or scroll the graph back through the last 20 minutes. There's also a **phase portrait** of prey against predators, where the loops of the Lotka–Volterra cycle show up as the simulation runs.

## Controls

- **Pause / Reset / Speed (1×, 2×, 4×)**
- **Sliders** for every parameter above. Starting counts apply when you reset.
- **Click the field** to drop in a prey or a predator. Choose which with the toggle.
- **Keyboard:** `Space` pauses, `R` resets. When the graph is focused, `←` / `→` scroll it and `End` jumps back to live.

## Built with

Plain HTML, CSS and JavaScript on a `<canvas>`, with no frameworks. Fonts are Bricolage Grotesque, Atkinson Hyperlegible and JetBrains Mono from Google Fonts.

## License

Released under the [MIT License](LICENSE). You're free to use, copy, modify and share this code, including in your own projects, as long as you keep the copyright notice.
