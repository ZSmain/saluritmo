<script>
	import { Button } from '$lib/components/ui/button';
	import { Slider } from '$lib/components/ui/slider';
	import { ThermometerSun, FlameIcon } from 'lucide-svelte';

	let hasFetchedData = false; // Tracks whether weather data has been fetched
	let recordedData = false; // Tracks whether data has been recorded
	let streak = 5; // Tracks the current streak

	async function fetchWeatherData() {
		hasFetchedData = true;
	}
</script>

<div class="flex flex-col h-full">
	<header class="bg-primary text-primary-foreground px-4 py-2 flex items-center justify-between">
		<div>
			<svg
				class="w-6 h-6 mr-2 inline-block align-middle fill-white"
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 308.446 308.446"
				xml:space="preserve"
			>
				<g>
					<g>
						<path
							d="M274.877,147.049c-21.14-28.826-45.214-46.036-64.397-46.036c-6.424,0-12.328,2.601-17.076,7.521
                c-3.788,3.926-6.804,9.284-9.203,15.715l-8.598-10.031V40.283c0-5.908-9.572-10.689-21.379-10.689
                c-11.807,0-21.379,4.781-21.379,10.689v73.935l-8.599,10.032c-2.4-6.43-5.416-11.789-9.204-15.714
                c-4.748-4.921-10.652-7.521-17.076-7.521c-19.184,0-43.257,17.21-64.396,46.036C12.549,175.712,0,208.653,0,235.166
                c0,23.272,33.097,43.688,70.826,43.688c39.64,0,61.029-22.509,61.029-43.688c0-9.452,2.953-54.14-1.926-87.262l24.293-28.342
                l24.294,28.343c-4.878,33.112-1.926,77.828-1.926,87.261c0,21.179,21.39,43.688,61.029,43.688
                c37.729,0,70.826-20.415,70.826-43.688C308.446,208.653,295.897,175.711,274.877,147.049z M116.855,235.166
                c0,13.907-16.132,28.688-46.029,28.688C38.441,263.853,15,246.717,15,235.166c0-23.438,11.464-53.063,30.665-79.246
                c17.778-24.242,38.308-39.906,52.301-39.906c2.385,0,4.322,0.906,6.281,2.938C122.643,138.017,116.855,222.627,116.855,235.166z
                 M237.62,263.853c-29.897,0-46.029-14.78-46.029-28.688c0-12.114-5.823-97.113,12.607-116.216c1.96-2.031,3.896-2.937,6.281-2.937
                c13.993,0,34.523,15.664,52.302,39.907c19.201,26.183,30.665,55.807,30.665,79.245
                C293.446,246.717,270.005,263.853,237.62,263.853z"
						/>
					</g>
					<g>
						<circle cx="46.03" cy="191.54" r="8.135" />
					</g>
					<g>
						<path
							d="M84.626,132.084c-0.792,0.39-19.552,9.877-34.105,37.677c-1.921,3.67-0.504,8.202,3.166,10.123
                c3.685,1.928,8.209,0.49,10.123-3.166c12.058-23.032,27.172-31.035,27.491-31.201c3.685-1.845,5.193-6.321,3.368-10.022
                C92.838,131.78,88.343,130.249,84.626,132.084z"
						/>
					</g>
				</g>
			</svg>
			<span class="font-medium text-lg">PulmoRitmo</span>
			<!-- {#if $user}
				<span class="text-sm text-muted-foreground ml-2">{$user.name}</span>
			{/if} -->
		</div>
		<div class="flex items-center gap-2">
			<div class="text-sm font-medium">
				<span class="inline-flex items-center gap-1">
					{#if streak}
						<FlameIcon class="w-4 h-4 text-yellow-500" />
					{:else}
						<FlameIcon class="w-4 h-4 text-gray-400" />
					{/if}
					{streak}
				</span>
			</div>
		</div>
	</header>
	<div class="flex-1 grid gap-6 p-4">
		{#if !hasFetchedData && !recordedData}
			<div class="flex flex-col justify-center items-center space-y-4">
				<p class="text-center text-lg font-medium text-muted-foreground">
					Record your daily data to keep track of your health
				</p>
				<Button
					class="w-24 h-24 rounded-full bg-primary text-primary-foreground shadow-lg shadow-primary/50"
					onclick={fetchWeatherData}
				>
					<ThermometerSun />
				</Button>
			</div>
		{:else}
			<div class="grid gap-4">
				<div class="grid gap-2">
					<div class="flex items-center justify-between">
						<div class="text-sm font-medium">Weather</div>
						<div class="text-sm text-muted-foreground">
							<span class="font-medium">24°C</span> | 65% | 10 mph
						</div>
					</div>
					<div class="grid gap-2">
						<div class="flex items-center justify-between">
							<div class="text-sm text-muted-foreground">Temperature</div>
							<div class="text-sm font-medium">24°C</div>
						</div>
						<div class="flex items-center justify-between">
							<div class="text-sm text-muted-foreground">Humidity</div>
							<div class="text-sm font-medium">65%</div>
						</div>
						<div class="flex items-center justify-between">
							<div class="text-sm text-muted-foreground">Wind</div>
							<div class="text-sm font-medium">10 mph</div>
						</div>
					</div>
				</div>
				<div class="grid gap-2">
					<div class="flex items-center justify-between">
						<div class="text-sm font-medium">Breathing</div>
						<div class="text-sm font-medium">80%</div>
					</div>
					<Slider value={[80]} max={100} step={1} />
				</div>
				<div class="grid gap-2">
					<div class="flex items-center justify-between">
						<div class="text-sm font-medium">Mucus</div>
						<div class="text-sm font-medium">30%</div>
					</div>
					<Slider value={[30]} max={100} step={1} />
				</div>
				<div class="grid gap-2">
					<div class="flex items-center justify-between">
						<div class="text-sm font-medium">Wheezing</div>
						<div class="text-sm font-medium">10%</div>
					</div>
					<Slider value={[10]} max={100} step={1} />
				</div>
				<div class="grid gap-2">
					<div class="flex items-center justify-between">
						<div class="text-sm font-medium">Coughing</div>
						<div class="text-sm font-medium">0%</div>
					</div>
					<Slider value={[0]} max={100} step={1} />
				</div>
			</div>
			<Button>Record Data</Button>
		{/if}
	</div>
</div>
