<script>
	let count = 0;

	function handleCount() {
		count++;
	}
</script>

<h1>Welcome to SvelteKit</h1>
<button on:click={handleCount}>Clicked {count} times</button>
