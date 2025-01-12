<script lang="ts">
	const { icon, title, isActive = false, onClick } = $props();
</script>

<button
	onclick={onClick}
	class="flex h-[30px] items-center gap-2 rounded-sm px-2 hover:bg-white/10 {isActive
		? 'bg-white/20'
		: ''}"
>
	<img src={icon} alt={title} class="h-4 w-4" />
	<span class="text-shadow text-xs text-white">{title}</span>
</button>
