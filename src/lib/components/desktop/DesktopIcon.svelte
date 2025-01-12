<script lang="ts">
	export let icon: string;
	export let label: string;
	export let onClick: () => void;
</script>

<button
	on:click={onClick}
	class="flex w-[75px] flex-col items-center rounded p-3 focus:bg-white/20"
>
	<img src={icon} alt={label} class="mb-1 h-8 w-8" />
	<span class="text-shadow break-words text-center font-departure text-xs text-white">
		{label}
	</span>
</button>

<style>
	.text-shadow {
		text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
	}
</style>
