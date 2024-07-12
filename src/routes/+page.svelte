<script>
	import '../global.css';
	import { setContext } from 'svelte';
	import { writable } from 'svelte/store';
	import WeatherBody from '../components/WeatherBody.svelte';
	import WeatherFooter from '../components/WeatherFooter.svelte';
	import SearchIcon from '$lib/search-icon.svg';

	export let weatherInfo;
	let search;
	let error = '';

	// Global state management
	// Declare a global writable variable
	const weather = writable();

	// Set the retrieved weather information to the writable variable
	$: weather.set(weatherInfo);

	// Set the writable variable to the global context
	setContext('weather', weather);

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
			<WeatherBody />
			<WeatherFooter />
		</div>
	{:else}
		<p class="text-lg p-2">Your search will appear here...</p>
	{/if}
</div>
