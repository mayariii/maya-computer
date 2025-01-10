<script lang="ts">
	import '../app.css';
	import '../lib/fonts.css';
	import "7.css";
	let { children } = $props();
</script>

<div class="h-dvh w-screen bg-neutral-900">
	{@render children()}
</div>

<style>
	/* Override specific 7.css styles that might leak */
	:global(button:not(.win7-elements button)) {
		/* Reset only Win7 button styles outside win7-elements */
		background: initial;
		border: initial;
		box-shadow: initial;
	}

	:global(input:not(.win7-elements input)) {
		/* Reset only Win7 input styles outside win7-elements */
		background: initial;
		border: initial;
		box-shadow: initial;
	}

	/* Add more specific overrides as needed */
</style>
