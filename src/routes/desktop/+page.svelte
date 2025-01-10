<script lang="ts">
	import Window from '$lib/components/desktop/Window.svelte';
	import DesktopIcon from '$lib/components/desktop/DesktopIcon.svelte';
	import paintIcon from '$lib/assets/icons/paint.ico';

	let time = $state(new Date());
	let showPaint = $state(false);

	// Format time as "h:mm AM/PM"
	let formattedTime = $derived(
		time.toLocaleTimeString('en-US', {
			hour: 'numeric',
			minute: '2-digit',
			hour12: true
		})
	);

	let intervalId: number;

	$effect(() => {
		// Update time every second
		intervalId = window.setInterval(() => {
			time = new Date();
		}, 1000);

		return () => {
			// Cleanup interval when component is destroyed
			if (intervalId) clearInterval(intervalId);
		};
	});

	function handlePaintClick() {
		showPaint = true;
	}

	function handlePaintClose() {
		showPaint = false;
		isPaintLoading = true;
	}

	let isPaintLoading = $state(true);

	function handlePaintLoad() {
		isPaintLoading = false;
		if (intervalId) clearInterval(intervalId);
	}
</script>

<div class="relative flex h-screen w-full flex-col bg-[#1d80ce]">
	<!-- Main desktop area -->
	<div class="desktop-area flex-1 p-4">
		<!-- Non-Win7 styled elements -->
		<DesktopIcon icon={paintIcon} label="Paint" onClick={handlePaintClick} />

		{#if showPaint}
			<!-- Win7 styled elements -->
			<div class="win7-elements">
				<Window
					title="Paint"
					initialX={50}
					initialY={50}
					zIndex={0}
					maxWidth="900px"
					maxHeight="1200px"
					onClose={handlePaintClose}
				>
					<div class="relative h-[600px] w-full">
						{#if isPaintLoading}
							<div class="absolute inset-0 flex items-center justify-center bg-[#f0f0f0]">
								<div class="flex w-[300px] flex-col gap-3 text-center">
									<img src={paintIcon} alt="Paint" class="mx-auto mb-2 h-12 w-12" />
									<div
										role="progressbar"
										class="marquee"
										aria-valuemin="0"
										aria-valuemax="100"
									>
									</div>
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
		{/if}
	</div>

	<!-- Non-Win7 styled taskbar -->
	<div
		class="flex h-[40px] w-full items-center bg-gradient-to-b from-[#255c8f] to-[#214c75] px-2 shadow-lg"
	>
		<button
			class="flex h-[30px] items-center rounded-sm bg-gradient-to-b from-[#407096] to-[#2d5c84] px-2 hover:from-[#4c82ac] hover:to-[#356a98]"
		>
			<svg class="mr-1 h-[20px] w-[20px]" viewBox="0 0 24 24">
				<path
					fill="currentColor"
					class="text-white"
					d="M 2 3 L 10 2 L 10 11 L 2 11 L 2 3 Z M 12 2 L 22 1 L 22 11 L 12 11 L 12 2 Z M 2 13 L 10 13 L 10 22 L 2 21 L 2 13 Z M 12 13 L 22 13 L 22 23 L 12 22 L 12 13 Z"
				/>
			</svg>
			<span class="text-shadow text-white">Start</span>
		</button>
	</div>
</div>

<style>
	.text-shadow {
		text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.5);
	}
</style>
