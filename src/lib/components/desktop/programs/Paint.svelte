<script lang="ts">
	import Window from '../Window.svelte';
	import paintIcon from '$lib/assets/icons/paint.ico';

	const { onClose } = $props();

	let isPaintLoading = $state(true);

	function handlePaintLoad() {
		isPaintLoading = false;
	}
</script>

<div class="win7-elements">
	<Window
		title="Paint"
		initialX={20}
		initialY={10}
		maxWidth="900px"
		maxHeight="1200px"
		{onClose}
	>
		<div class="relative h-[600px] w-full">
			{#if isPaintLoading}
				<div class="absolute inset-0 flex items-center justify-center bg-[#f0f0f0]">
					<div class="flex w-[300px] flex-col gap-3 text-center">
						<img src={paintIcon} alt="Paint" class="mx-auto mb-2 h-12 w-12" />
						<div role="progressbar" class="marquee" aria-valuemin="0" aria-valuemax="100"></div>
					</div>
				</div>
			{/if}
			<iframe
				src="https://jspaint.app#theme:modern.css"
				id="jspaint-iframe"
				width="100%"
				height="100%"
				title="Paint"
				onload={handlePaintLoad}
			></iframe>
		</div>
	</Window>
</div>
