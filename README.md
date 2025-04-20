# Conway's Game of Life

An interactive implementation of Conway's Game of Life using Svelte and Tailwind CSS.

## Features

- Responsive grid that adapts to window size
- Interactive cell editing (click and drag to draw patterns)
- Adjustable simulation speed
- Pre-built patterns (Glider, Pulsar)
- Status display showing grid dimensions and generation count
- Modern glass-effect UI

## Controls

- **Start/Stop**: Toggle the simulation
- **Step**: Advance one generation when paused
- **Clear**: Reset the grid to empty
- **Random**: Fill the grid with a random pattern
- **Add Glider/Pulsar**: Add pre-built patterns
- **Speed**: Adjust simulation speed (Slow/Normal/Fast)

## Development

Once you've cloned the project and installed dependencies with `npm install`, start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

## About Conway's Game of Life

Conway's Game of Life is a cellular automaton devised by mathematician John Conway in 1970. It follows simple rules:

1. Any live cell with 2 or 3 live neighbors survives
2. Any dead cell with exactly 3 live neighbors becomes alive
3. All other cells die or remain dead

These simple rules create complex emergent patterns and behaviors.

## Implementation Details

- Built with Svelte
- Uses Tailwind CSS for styling
- Implements wrap-around grid edges
