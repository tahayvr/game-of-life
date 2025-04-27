<script lang="ts">
	import * as Card from '$lib/components/ui/card/index.js';

	export let generationCount: number;
	export let rows: number;
	export let cols: number;
	export let cellSize: number;
	export let running: boolean;
	export let speed: number;

	$: generationsPerSecond = speed > 0 ? Math.round(1000 / speed) : 0;
</script>

<div class="fixed left-0 right-0 top-4 z-10 hidden justify-center md:flex">
	<Card.Root class="border-0 bg-background/50 shadow-lg backdrop-blur-sm">
		<Card.Content class="py-3">
			<div class="flex flex-wrap items-center justify-center gap-4">
				<span class="text-sm">Generation: {generationCount}</span>
				<span class="text-sm">Grid: {rows}x{cols}</span>
				<span class="text-sm">Cell Size: {cellSize}px</span>
				<span class="text-sm">Status: {running ? 'Running' : 'Paused'}</span>
				<span class="text-sm">Speed: {generationsPerSecond} gen/sec</span>
			</div>
		</Card.Content>
	</Card.Root>
</div>
