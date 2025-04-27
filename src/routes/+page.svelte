<script>
	import Conway from '$lib/components/Conway.svelte';
	import Info from '$lib/components/Info.svelte';
	import SvelteSeo from 'svelte-seo';
</script>

<main class="m-0 h-screen w-screen overflow-hidden p-0">
	<Conway />
	<Info />
</main>

<SvelteSeo
	title="Game of Life | by noiseRandom"
	description="Conway's Game of Life is a cellular automaton devised by the British mathematician John Conway."
	keywords="game of life, cellular automaton, noiseRandom"
	canonical="https://gameoflife.taha.gg"
	openGraph={{
		title: 'Game of Life | by noiseRandom',
		description:
			"Conway's Game of Life is a cellular automaton devised by the British mathematician John Conway.",
		url: 'https://gameoflife.taha.gg',
		type: 'website',
		images: [
			{
				url: 'https://gameoflife.taha.gg/images/gameoflife-noiserandom.webp',
				width: 800,
				height: 600
			}
		],
		site_name: 'Game of Life | by noiseRandom'
	}}
	twitter={{
		card: 'summary_large_image',
		site: '@tahayvr',
		title: 'Game of Life | by noiseRandom',
		description:
			"Conway's Game of Life is a cellular automaton devised by the British mathematician John Conway.",
		image: 'https://gameoflife.taha.gg/images/gameoflife-noiserandom.webp'
	}}
/>
