<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import { Separator } from '$lib/components/ui/separator/index.js';

	export let running: boolean;
	export let speed: number;

	export let toggleRunning: () => void;
	export let updateGrid: () => void;
	export let clearGrid: () => void;
	export let randomizeGrid: () => void;
	export let addGlider: (row: number, col: number) => void;
	export let addPulsar: (row: number, col: number) => void;
	export let setSpeed: (speed: number) => void;
</script>

<div class="fixed bottom-4 left-0 right-0 z-10 flex justify-center">
	<Card.Root class="border-0 bg-background/50 shadow-lg backdrop-blur-sm">
		<Card.Content class="py-3">
			<div class="flex flex-nowrap items-center justify-center gap-2 overflow-x-auto px-2">
				<Button variant={running ? 'destructive' : 'default'} onclick={toggleRunning}>
					{running ? 'Stop' : 'Start'}
				</Button>

				<Button variant="outline" onclick={updateGrid} disabled={running}>Step</Button>
				<Button variant="outline" onclick={clearGrid}>Clear</Button>
				<Separator orientation="vertical" class="mx-2" />
				<Button variant="outline" onclick={randomizeGrid}>Random</Button>
				<Button variant="outline" onclick={() => addGlider(5, 5)}>Add Glider</Button>
				<Button variant="outline" onclick={() => addPulsar(5, 20)}>Add Pulsar</Button>
				<Separator orientation="vertical" class="mx-2" />
				<span class="whitespace-nowrap text-sm">Speed:</span>
				<Button
					variant={speed >= 300 ? 'default' : 'outline'}
					size="sm"
					onclick={() => setSpeed(500)}
				>
					Slow
				</Button>
				<Button
					variant={speed >= 80 && speed < 300 ? 'default' : 'outline'}
					size="sm"
					onclick={() => setSpeed(100)}
				>
					Normal
				</Button>
				<Button variant={speed < 80 ? 'default' : 'outline'} size="sm" onclick={() => setSpeed(50)}>
					Fast
				</Button>
			</div>
		</Card.Content>
	</Card.Root>
</div>
