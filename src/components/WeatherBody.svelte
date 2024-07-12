<script>
	import { getContext } from 'svelte';
	import LocationIcon from '$lib/location.png';

	// Get the weather from the global context
	const weather = getContext('weather');
</script>

<img src={$weather.current.condition.icon} width="80px" alt="Condition Icon" />
<p class="text-3xl font-semibold">{$weather.current.temp_c}°C</p>
<p class="text-lg mb-2">{$weather.current.condition.text}</p>
<div class="flex items-center gap-1 p-2">
	<img src={LocationIcon} width="18px" height="18px" alt="Location Icon" />
	<p class="text-lg">{$weather.location.name}, {$weather.location.region}</p>
</div>
