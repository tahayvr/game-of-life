<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { browser } from '$app/environment';
	import ConwayControls from '$lib/components/ConwayControls.svelte';
	import ConwayStatusBar from '$lib/components/ConwayStatusBar.svelte';

	// Grid size
	export let cellSize = 12; // Base cell size
	let rows = 50;
	let cols = 70;

	// Responsive sizing
	let containerWidth: number;
	let containerHeight: number;
	let gridContainer: HTMLDivElement;
	let minCellSize = 12; // Increased minimum size for better visibility

	// Mouse tracking for drag selection
	let isMouseDown = false;
	let drawMode = true; // true = drawing cells, false = erasing cells

	// Calculate grid dimensions and cell size based on container
	function calculateGrid() {
		if (!browser || !gridContainer) return;

		// Get available space (full window)
		containerWidth = window.innerWidth;
		containerHeight = window.innerHeight; // Use the full height

		// Calculate how many cells we can fit
		cols = Math.floor(containerWidth / minCellSize);
		rows = Math.floor(containerHeight / minCellSize);

		// Calculate the actual cell size to fit perfectly
		cellSize = Math.floor(Math.min(containerWidth / cols, containerHeight / rows));

		// If we already have a grid, resize it
		if (grid.length > 0) {
			resizeGrid(rows, cols);
		} else {
			initGrid();
		}
	}

	// Resize the grid while preserving existing cell states where possible
	function resizeGrid(newRows: number, newCols: number) {
		const oldGrid = [...grid];
		const oldRows = oldGrid.length;
		const oldCols = oldRows > 0 ? oldGrid[0].length : 0;

		// Create new grid with the new dimensions
		const newGrid = Array(newRows)
			.fill(null)
			.map(() => Array(newCols).fill(false));

		// Copy existing cell states where they overlap
		for (let i = 0; i < Math.min(oldRows, newRows); i++) {
			for (let j = 0; j < Math.min(oldCols, newCols); j++) {
				newGrid[i][j] = oldGrid[i][j];
			}
		}

		grid = newGrid;
	}

	// Game state
	let grid: boolean[][] = [];
	let running = false;
	let generationCount = 0;
	let intervalId: ReturnType<typeof setInterval> | null = null;
	let speed = 100; // ms between generations

	// Initialize grid
	function initGrid() {
		grid = Array(rows)
			.fill(null)
			.map(() => Array(cols).fill(false));
		generationCount = 0;
	}

	// Randomize grid (approximately 25% alive)
	function randomizeGrid() {
		grid = Array(rows)
			.fill(null)
			.map(() =>
				Array(cols)
					.fill(false)
					.map(() => Math.random() < 0.25)
			);
		generationCount = 0;
	}

	// Reset grid to empty
	function clearGrid() {
		initGrid();
		if (running) toggleRunning();
	}

	// Toggle cell state
	function toggleCell(row: number, col: number) {
		if (!running) {
			grid[row][col] = !grid[row][col];
			grid = [...grid];
		}
	}

	// Set cell state based on draw mode
	function setCell(row: number, col: number, state: boolean) {
		if (!running && row >= 0 && row < rows && col >= 0 && col < cols) {
			grid[row][col] = state;
			grid = [...grid];
		}
	}

	// Handle mouse down on a cell
	function handleMouseDown(row: number, col: number) {
		if (running) return;

		isMouseDown = true;
		drawMode = !grid[row][col]; // If cell is alive, we'll erase. If dead, we'll draw.
		setCell(row, col, drawMode);
	}

	// Handle mouse enter on a cell during drag
	function handleMouseEnter(row: number, col: number) {
		if (isMouseDown && !running) {
			setCell(row, col, drawMode);
		}
	}

	// Handle mouse up event
	function handleMouseUp() {
		isMouseDown = false;
	}

	// Count neighbors for a cell
	function countNeighbors(row: number, col: number) {
		let count = 0;

		for (let i = -1; i <= 1; i++) {
			for (let j = -1; j <= 1; j++) {
				if (i === 0 && j === 0) continue;

				const newRow = (row + i + rows) % rows;
				const newCol = (col + j + cols) % cols;

				if (grid[newRow][newCol]) count++;
			}
		}

		return count;
	}

	// Update grid based on Conway's rules
	function updateGrid() {
		const newGrid = Array(rows)
			.fill(null)
			.map(() => Array(cols).fill(false));

		for (let row = 0; row < rows; row++) {
			for (let col = 0; col < cols; col++) {
				const neighbors = countNeighbors(row, col);
				const isAlive = grid[row][col];

				if (isAlive) {
					// Any live cell with 2 or 3 live neighbors survives
					newGrid[row][col] = neighbors === 2 || neighbors === 3;
				} else {
					// Any dead cell with exactly 3 live neighbors becomes a live cell
					newGrid[row][col] = neighbors === 3;
				}
			}
		}

		grid = newGrid;
		generationCount++;
	}

	// Start/stop simulation
	function toggleRunning() {
		running = !running;

		if (running) {
			intervalId = setInterval(updateGrid, speed);
		} else if (intervalId) {
			clearInterval(intervalId);
			intervalId = null;
		}
	}

	// Change simulation speed
	function setSpeed(newSpeed: number) {
		speed = newSpeed;

		if (running && intervalId) {
			clearInterval(intervalId);
			intervalId = setInterval(updateGrid, speed);
		}
	}

	// Preset patterns
	function addGlider(startRow: number, startCol: number) {
		if (running) return;

		for (let i = 0; i < 3; i++) {
			for (let j = 0; j < 3; j++) {
				const row = (startRow + i) % rows;
				const col = (startCol + j) % cols;
				grid[row][col] = false;
			}
		}

		// glider
		grid[(startRow + 0) % rows][(startCol + 1) % cols] = true;
		grid[(startRow + 1) % rows][(startCol + 2) % cols] = true;
		grid[(startRow + 2) % rows][(startCol + 0) % cols] = true;
		grid[(startRow + 2) % rows][(startCol + 1) % cols] = true;
		grid[(startRow + 2) % rows][(startCol + 2) % cols] = true;

		grid = [...grid];
	}

	function addPulsar(startRow: number, startCol: number) {
		if (running) return;

		// Pulsar pattern (period 3 oscillator)
		const pattern = [
			[0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
			[0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
			[0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
			[0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0],
			[0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
			[0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
			[0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0]
		];

		for (let i = 0; i < pattern.length; i++) {
			for (let j = 0; j < pattern[0].length; j++) {
				const row = (startRow + i) % rows;
				const col = (startCol + j) % cols;
				grid[row][col] = pattern[i][j] === 1;
			}
		}

		grid = [...grid];
	}

	function handleResize() {
		calculateGrid();
	}

	onMount(() => {
		if (browser) {
			setTimeout(() => {
				calculateGrid();
				window.addEventListener('resize', handleResize);
				window.addEventListener('mouseup', handleMouseUp);
			}, 100);
		} else {
			initGrid();
		}

		return () => {
			if (intervalId) clearInterval(intervalId);
			if (browser) {
				window.removeEventListener('resize', handleResize);
				window.removeEventListener('mouseup', handleMouseUp);
			}
		};
	});

	onDestroy(() => {
		if (intervalId) clearInterval(intervalId);
		if (browser) {
			window.removeEventListener('resize', handleResize);
			window.removeEventListener('mouseup', handleMouseUp);
		}
	});
</script>

<main class="h-screen w-full overflow-hidden" aria-label="Conway's Game of Life">
	<div
		class="relative h-full w-full"
		onmouseup={handleMouseUp}
		onmouseleave={handleMouseUp}
		tabindex="-1"
		aria-label="Game of Life Grid"
		role="grid"
	>
		<div class="absolute inset-0 flex">
			<div
				bind:this={gridContainer}
				class="grid-container h-full w-full"
				role="grid"
				aria-label="Game of Life Grid"
			>
				{#each grid as row, rowIndex}
					<div class="flex h-auto" role="row">
						{#each row as cell, colIndex}
							<button
								type="button"
								class="cell {cell ? 'alive' : 'dead'}"
								style="width: {cellSize}px; height: {cellSize}px;"
								onmousedown={() => handleMouseDown(rowIndex, colIndex)}
								onmouseenter={() => handleMouseEnter(rowIndex, colIndex)}
								disabled={running}
								aria-label={`Cell at row ${rowIndex}, column ${colIndex}`}
								role="gridcell"
							></button>
						{/each}
					</div>
				{/each}
			</div>
		</div>

		<ConwayStatusBar {generationCount} {rows} {cols} {cellSize} {running} {speed} />

		<ConwayControls
			{running}
			{speed}
			{toggleRunning}
			{updateGrid}
			{clearGrid}
			{randomizeGrid}
			{addGlider}
			{addPulsar}
			{setSpeed}
		/>
	</div>
</main>

<style>
	.cell {
		@apply m-0 border border-gray-200 p-0;
		transition: background-color 0.1s ease;
		user-select: none;
		cursor: pointer;
	}

	.alive {
		@apply bg-black;
	}

	.dead {
		@apply bg-background hover:bg-gray-200;
	}

	.grid-container {
		max-width: 100%;
		max-height: 100%;
		overflow: hidden;
		display: inline-block;
	}
</style>
