<script>
	import '../global.css';
	import SearchIcon from '$lib/search-icon.svg';
	import LocationIcon from '$lib/location.png';
	import TemperatureIcon from '$lib/temperature.png';
	import HumidityIcon from '$lib/humidity.png';

	let weatherInfo;
	let search;
	let error = '';

	async function fetchData() {
		const url = `https://weatherapi-com.p.rapidapi.com/current.json?q=${search}`;
		const options = {
			method: 'GET',
			headers: {
				'x-rapidapi-key': '920f68d20amsh149e76c42dfa7a7p1da854jsn948a14786ccb',
				'x-rapidapi-host': 'weatherapi-com.p.rapidapi.com'
			}
		};

		try {
			const response = await fetch(url, options);
			const result = await response.json();

			if (response.ok) {
				weatherInfo = { location: result.location, current: result.current };
				error = '';
			} else {
				weatherInfo = null;
				error = result.error.message || 'City not found. Please try again.';
			}
		} catch (err) {
			console.error(err);
			weatherInfo = null;
			error = 'Something went wrong. Please try again.';
		}
	}
</script>

<link
	href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap"
	rel="stylesheet"
/>

<div class="bg-[#fcfcfc] w-96 h-auto rounded-md flex flex-col items-center m-auto">
	<p class="p-2 border-slate-300 border-b-2 w-full text-center font-semibold">Search for a city</p>
	<form on:submit={() => fetchData()} class="flex gap-1 relative">
		<input
			bind:value={search}
			required
			type="text"
			placeholder="Enter city name"
			class="text-center my-2.5 border-2 rounded-md focus:border-[#63b5d8] focus:outline-none"
		/>
		<button type="submit"><img src={SearchIcon} alt="Search Icon" width="23px" /></button>
	</form>

	{#if error}
		<p class="text-lg p-2 text-red-600">{error}</p>
	{:else if weatherInfo}
		<div class="flex flex-col items-center w-full">
			<img src={weatherInfo.current.condition.icon} width="80px" alt="Condition Icon" />
			<p class="text-3xl font-semibold">{weatherInfo.current.temp_c}°C</p>
			<p class="text-lg mb-2">{weatherInfo.current.condition.text}</p>
			<div class="flex items-center gap-1 p-2">
				<img src={LocationIcon} width="18px" height="18px" alt="Location Icon" />
				<p class="text-lg">{weatherInfo.location.name}, {weatherInfo.location.region}</p>
			</div>
			<div class="flex items-center justify-evenly border-slate-300 border-t-2 w-full">
				<div class="flex p-2">
					<img src={TemperatureIcon} height="20px" alt="Temperature Icon" />
					<div>
						<p class="text-lg font-semibold">{weatherInfo.current.feelslike_c}°C</p>
						<p class="text-sm">Feels like</p>
					</div>
				</div>
				<div class="border-l border-slate-300 h-20"></div>
				<div class="flex p-2">
					<img src={HumidityIcon} width="45px" height="18px" alt="Temperature Icon" />
					<div>
						<p class="text-lg font-semibold">{weatherInfo.current.humidity}%</p>
						<p class="text-sm">Humidity</p>
					</div>
				</div>
			</div>
		</div>
	{:else}
		<p class="text-lg p-2">Your search will appear here...</p>
	{/if}
</div>
