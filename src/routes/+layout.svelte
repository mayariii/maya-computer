<script lang="ts">
	import '../app.css';
	import '../lib/fonts.css';
	
	let { children } = $props();
</script>

<div class="h-dvh w-screen bg-neutral-900">
	{@render children()}

</div>
