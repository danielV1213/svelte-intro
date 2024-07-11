<script>
	import { onMount } from 'svelte';
	import '../global.css';
	import SearchIcon from '$lib/search-icon.svg';

	let location = {};
	let current = {};
	let condition = {};

	async function fetchData() {
		const url = 'https://weatherapi-com.p.rapidapi.com/current.json?q=Medellin';
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
			console.log(result.current);
			location = result.location;
			current = result.current;
			condition = result.current.condition;
			console.log(result);
		} catch (error) {
			console.error(error);
		}
	}

	onMount(async () => {
		fetchData();
	});
	
</script>

<link
	href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap"
	rel="stylesheet"
/>

<section class="bg-[#fcfcfc] w-80 h-auto rounded-md flex flex-col items-center m-auto">
	<p class="p-2 border-slate-300 border-b-2 w-full text-center font-semibold">Search for a city</p>
	<div class="flex gap-2">
		<input
			type="text"
			placeholder="Enter city name"
			class="text-center my-2.5 border-2 rounded-md focus:border-[#63b5d8] focus:outline-none"
		/>
		<button><img src={SearchIcon} alt="Search Icon" width="23px" /></button>
	</div>

	<div class="flex flex-col items-center">
		<img src={condition.icon} width="75px" alt="Condition Icon">
		<p class="text-2xl font-semibold">{current.temp_c}</p>
		<p>{location.name}, {location.region}</p>
	</div>
</section>
