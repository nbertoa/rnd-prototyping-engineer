# Brief 002 — video shot list (60–90 s)

Record in the next build week. Engine: UE 5.8 Sandbox. Target length: ~75 s. Burned-in captions (one short line per shot), no voice-over required.

Draft post: https://nbertoa.com/?p=9856 (embed the video where the draft note says, then delete the note).

## Shots

| # | Time | Level | What to show | Caption |
|---|------|-------|--------------|---------|
| 1 | 0–6 s | `L_EnvironmentTest` | Wide shot of the scene with the sky running; title over it | "My UE 5.8 R&D plugin stack — and how I choose" |
| 2 | 6–14 s | — (editor) | Plugins window filtered to the stack, quick scroll | "Native first. Plugin second. Custom last." |
| 3 | 14–24 s | `L_EnvironmentTest` | Walk to the cube and campfire, interact with E and then gamepad Square | "Interaction: Nice Interaction System V2" |
| 4 | 24–32 s | `L_EnvironmentTest` | Interact with the test NPC/cube, dialogue opens, pick an option, the cube reacts | "Narrative: Narrative Tales" |
| 5 | 32–42 s | `L_EnvironmentTest` | Jump and sprint with the stamina bar draining and regenerating | "Gameplay: GAS Companion (abilities, costs, regen)" |
| 6 | 42–52 s | `L_EnvironmentTest` | Cycle Near / Medium / Far presets, then lock on a target with Focus | "Cameras: Ultimate Gameplay Camera" |
| 7 | 52–62 s | `L_Sky_BlueDawn` (+ any finished sky level) | Slow pan of the blue-hour sky; cut to a second look if ready | "Atmosphere: Ultra Dynamic Sky + fog scattering" |
| 8 | 62–70 s | `L_ChameleonTest` | Toggle a Chameleon effect on and off | "Perception: Chameleon Post Process" |
| 9 | 70–78 s | — | Black card with the closing line and the URL | "Full write-up: nbertoa.com" |

## Transitions

- Quick cross-dissolves (6–10 frames) between levels; hard cuts inside the same level.
- Keep the camera moving slowly in every shot so cuts feel continuous.

## Capture checklist

- [ ] 1920×1080 or 2560×1440, 60 fps; editor UI scaled so text reads on a phone
- [ ] Packaged build preferred for gameplay shots (no editor chrome)
- [ ] Hide personal paths and any API keys
- [ ] Raw clips to `media/raw/` (git-ignored)
- [ ] Thumbnail: shot 6 or 7 frame + "My UE5 R&D stack"
- [ ] Upload to YouTube, embed in the draft, delete the draft note, then publish
