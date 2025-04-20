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
				<InfoIcon />
			</Button>
		</Dialog.Trigger>
		<Dialog.Content>
			<Dialog.Header>
				<Dialog.Title>Conway's Game of Life</Dialog.Title>
				<Dialog.Description>
					<div class="mt-4 flex flex-col gap-2">
						<p>
							Conway's Game of Life is a cellular automaton devised by the British mathematician
							John Conway. It is a zero-player game, meaning that its evolution is determined by its
							initial state, requiring no further input.
						</p>
						<p>
							One interacts with the Game of Life by creating an initial configuration and observing
							how it evolves. It is Turing complete and can simulate a universal constructor or any
							other Turing machine.
						</p>
						<Separator class="my-4" />
						<p class="self-center text-sm text-muted-foreground">
							© {year} | Taha AKA noiseRandom
						</p>
					</div>
				</Dialog.Description>
			</Dialog.Header>
		</Dialog.Content>
	</Dialog.Root>
</div>
