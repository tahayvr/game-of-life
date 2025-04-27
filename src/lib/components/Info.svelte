<script>
	import * as Dialog from '$lib/components/ui/dialog/index.js';
	import { Button } from '$lib/components/ui/button/index.js';
	import InfoIcon from '@lucide/svelte/icons/info';
	import { Separator } from '$lib/components/ui/separator/index.js';

	const year = new Date().getFullYear();
</script>

<div class="absolute left-4 top-4 z-10">
	<Dialog.Root>
		<Dialog.Trigger>
			<Button variant="outline" size="icon" class="bg-background/50 shadow-lg backdrop-blur-sm">
				<InfoIcon class="h-4 w-4" />
			</Button>
		</Dialog.Trigger>
		<Dialog.Content>
			<Dialog.Header>
				<Dialog.Title>Conway's Game of Life</Dialog.Title>
				<Dialog.Description>
					<div class="mt-4 flex flex-col gap-2">
						<p>
							Conway's Game of Life is a cellular automaton devised by the British mathematician
							John Conway. It follows simple rules:
						</p>
						<ol class="list-decimal pl-4">
							<li>
								Any live cell with fewer than two live neighbours dies, as if by underpopulation.
							</li>
							<li>
								Any live cell with two or three live neighbours lives on to the next generation.
							</li>
							<li>
								Any live cell with more than three live neighbours dies, as if by overpopulation.
							</li>
							<li>
								Any dead cell with exactly three live neighbours becomes a live cell, as if by
								reproduction.
							</li>
						</ol>
						<p>
							One interacts with the Game of Life by creating an initial configuration and observing
							how it evolves.
						</p>
						<Separator class="my-4" />
						<p class="self-center text-sm text-muted-foreground">
							© {year} | <a href="https://taha.gg">noiseRandom</a>
						</p>
					</div>
				</Dialog.Description>
			</Dialog.Header>
		</Dialog.Content>
	</Dialog.Root>
</div>
