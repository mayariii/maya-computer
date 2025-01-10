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

	:global(.no-win7) {
		/* Reset all Win7 styles for elements with this class */
		background: initial !important;
		border: initial !important;
		box-shadow: initial !important;
		border-radius: initial !important;
		padding: initial !important;
		margin: initial !important;
		color: initial !important;
		width: initial !important;
		height: initial !important;
		min-width: initial !important;
		min-height: initial !important;
		max-width: initial !important;
		max-height: initial !important;
		line-height: initial !important;
		text-align: initial !important;
		vertical-align: initial !important;

	}

	
</style>
