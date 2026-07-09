# NeuroForge Suite

NeuroForge Suite is a single-file web application for training focus, memory, and cognitive flexibility through a set of short, adaptive brain games. The experience combines a calm, minimal interface with game-based progression, streak tracking, and daily challenges.

## What the app does

The app presents five cognitive exercises:

- Dual N-Back for working memory and fluid intelligence
- Stroop Task for selective attention and inhibition
- Corsi Block-Tapping for visuospatial memory
- Trail Making Test Part B for cognitive flexibility
- Eriksen Flanker Task for conflict resolution and focus

Each game contributes to the player’s progression through experience points, levels, and streak-based rewards.

## Features

- Single-file app built with plain HTML, CSS, and JavaScript
- Adaptive difficulty and reward loops
- Daily focus challenge with bonus BXP
- Persistent progress using browser local storage
- Responsive dashboard and game sandbox UI
- Summary overlay after each completed session

## How to run it

Open [index.html](index.html) in a browser.

No installation or build step is required.

## Project structure

- [index.html](index.html) — complete app UI, styling, and game logic
- [explainer.md](explainer.md) — architecture and JavaScript explainer
- [README.md](README.md) — app overview and usage notes

## Architecture overview

The app is organized around:

- `AppState` for global player and UI state
- `GameManager` for routing between screens and sessions
- Individual JavaScript classes for each game module
- Local storage for progress persistence

## Notes

The app is intentionally self-contained so it can be run anywhere that supports a modern browser. It does not depend on external libraries, frameworks, or CDNs.

## Future ideas

Possible extensions include:

- richer sound and haptic feedback,
- more games and difficulty modes,
- leaderboards and social challenges,
- unlockable visual themes,
- achievement badges and milestones.
