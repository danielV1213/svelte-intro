<script>
	import { getContext } from 'svelte';
	import TemperatureIcon from '$lib/temperature.png';
	import HumidityIcon from '$lib/humidity.png';

	const weather = getContext('weather');
</script>

<div class="flex items-center justify-evenly border-slate-300 border-t-2 w-full">
	<div class="flex p-2">
		<img src={TemperatureIcon} height="20px" alt="Temperature Icon" />
		<div>
			<p class="text-lg font-semibold">{$weather.current.feelslike_c}°C</p>
			<p class="text-sm">Feels like</p>
		</div>
	</div>
	<div class="border-l border-slate-300 h-20"></div>
	<div class="flex p-2">
		<img src={HumidityIcon} width="45px" height="18px" alt="Temperature Icon" />
		<div>
			<p class="text-lg font-semibold">{$weather.current.humidity}%</p>
			<p class="text-sm">Humidity</p> 
		</div>
	</div>
</div>
