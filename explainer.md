# NeuroForge Suite — JavaScript Architecture Explainer

## Overview

NeuroForge Suite is a single-file web application built with plain HTML, CSS, and JavaScript. The entire experience lives in one document, [index.html](index.html), so it is easy to run, share, and modify without a build step.

## Core Architecture

The app uses a simple but structured architecture built around three main ideas:

1. A global state object to hold player progress and UI state.
2. A central game manager to route between the dashboard and active game sessions.
3. Modular game classes for each cognitive task.

### 1. Global App State

The app keeps a single object named `AppState` that stores:

- Player profile data such as level, total BXP, streak, and daily focus assignment.
- Best scores for each game.
- UI state such as which game is active and whether the app is in dashboard or sandbox mode.

This object is used across the app so the games and the header can stay in sync with the same player data.

### 2. Game Manager

The `GameManager` object acts as the app’s main controller.

It is responsible for:

- Initializing the app on load.
- Registering each game module.
- Rendering the dashboard of available games.
- Opening a selected game in the sandbox view.
- Handling game completion and reward distribution.
- Saving to `localStorage` and updating the header UI.

### 3. Modular Game Classes

Each game is implemented as its own JavaScript class:

- `DualNBackGame`
- `StroopGame`
- `CorsiGame`
- `TrailGame`
- `FlankerGame`

Each class owns the logic for its gameplay loop, its UI rendering, its scoring rules, and its completion flow. This keeps the code organized and makes it easier to expand the app later.

## How the Game Loop Works

Each game follows a similar lifecycle:

1. `mount(container, onComplete)`
   - Creates the DOM structure for the game.
   - Connects user interaction handlers.
   - Stores a completion callback for when the session ends.

2. `start()`
   - Resets the game session.
   - Initializes the trial or round state.
   - Begins the game play loop.

3. `stop()`
   - Stops timers or intervals.
   - Prevents the game from continuing after the session ends.

4. `finish()`
   - Computes the final score, accuracy, and level data.
   - Calls back to `GameManager` so the app can award BXP and show the summary overlay.

## Game-Specific Logic

### Dual N-Back

This game presents a series of position and audio cues across 20 trials.

The logic includes:

- Random placement of a highlighted square in a 3x3 grid.
- Synthetic speech playback using `window.speechSynthesis`.
- A comparison against the previous trial or the trial from two steps ago depending on the current `N` value.
- Adaptive progression based on accuracy.

### Stroop Task

This game measures selective attention and inhibition.

The logic includes:

- Displaying words with conflicting ink colors.
- A countdown timer per word.
- User input that must match the physical color rather than the word meaning.
- Progressive speed pressure as the player performs well.

### Corsi Block-Tapping

This game tests visuospatial memory.

The logic includes:

- A sequence of tiles flashing in order.
- The player repeating the sequence in the correct order.
- Lives and reset behavior when incorrect input occurs.
- Increasing sequence length as the player succeeds.

### Trail Making Test B

This game is a spatial reasoning and cognitive flexibility task.

The logic includes:

- Randomly positioned nodes labeled with alternating numbers and letters.
- The player clicking them in the correct numeric/letter alternation order.
- A timer that tracks session pace.
- Penalized errors with a time penalty.

### Eriksen Flanker Task

This game tests conflict resolution and focus.

The logic includes:

- A row of arrows with a central target arrow.
- Congruent and incongruent trials.
- The player choosing the direction indicated by the middle arrow only.
- A survival timer that rewards fast correct answers and penalizes mistakes.

## Progression and Rewards

The progression layer ties performance to long-term engagement.

### BXP and Leveling

At the end of a session, the app calculates BXP using this formula:

- BXP = (Base Score × Accuracy Percentage) + (Level Reached × 10)

It then updates the player’s total BXP and derives the new level using:

- Level = floor(sqrt(BXP / 100)) + 1

### Daily Focus

On first launch of the day, the app randomly selects one of the five games as the daily focus challenge.

If that game is played, it receives a 1.5x BXP multiplier.

### Streak Handling

The app tracks the current active streak by comparing the day of the last session with the current date. If the player returns the following day, the streak continues. If the streak is broken, it resets.

## Persistence

The app stores progress in the browser using `localStorage`.

The save logic writes the player state whenever:

- a game completes,
- a reward is earned,
- or progress changes.

This means the player can return later and continue from their previous state.

## UI Rendering Strategy

The UI is not built with a framework. Instead, the app uses direct DOM manipulation.

The interface is structured as:

- A top header with stats and progress.
- A dashboard showing all available games.
- A sandbox view for the active game.
- An overlay summary modal after each game session.

This keeps the app lightweight and fast while still supporting a polished experience.

## Why This Architecture Works

This architecture is intentionally simple:

- It avoids external dependencies.
- It keeps logic easy to read and modify.
- It supports a wide variety of game types under one consistent system.
- It is portable because it runs directly in the browser.

## Future Extension Ideas

This foundation makes it straightforward to add:

- more cognitive games,
- achievements and unlocks,
- sound effects and richer feedback,
- leaderboards or cloud sync,
- animated onboarding and tutorials.
