<script lang="ts">
	import { goto } from "$app/navigation";
	import Screensaver from "$lib/components/desktop/Screensaver.svelte";
</script>
	<Screensaver onClose={() => (goto("/desktop"))} />
