<script>
	import { ProgressRadial } from '@skeletonlabs/skeleton';
	import { onMount } from 'svelte';
	let original_time = 360;
	let value = 60; // %
	$: running = false;
	$: seconds = original_time;
	$: date = new Date(seconds * 1000).toISOString().substring(11, 19);
	$: percent_complete = (seconds / original_time) * 100;

	// if less than 1 day, only display hours & minutes
	$: if (seconds < 3600) {
		date = new Date(seconds * 1000).toISOString().substring(14, 19);
	}

	onMount(async () => {
		window.setInterval(() => {
			if (running) {
				seconds--;
			}
			console.log(percent_complete);
		}, 1000);
	});

	function startStop() {
		running = !running;
	}

	function resetTimer() {
		seconds = original_time;
		running = false;
	}
</script>

<!-- https://www.skeleton.dev/components/progress-radials -->

<div class="h-screen flex flex-col items-center justify-center">
	<ProgressRadial class="w-20" track="stroke-primary-500/30" value={percent_complete}
		>{date}</ProgressRadial
	>
	<h1>{date}</h1>
	<div>
		<button
			class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 w-20"
			on:click={startStop}
		>
			{running ? 'Stop' : 'Start'}
		</button>
		<button
			class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 w-20"
			on:click={resetTimer}
		>
			Reset
		</button>
	</div>
</div>
