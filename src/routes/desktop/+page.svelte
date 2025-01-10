<script lang="ts">
	import DesktopIcon from '$lib/components/desktop/DesktopIcon.svelte';
	import TaskbarButton from '$lib/components/desktop/TaskbarButton.svelte';
	import paintIcon from '$lib/assets/icons/paint.ico';
	import messengerIcon from '$lib/assets/icons/messenger.ico';
	import mediaIcon from '$lib/assets/icons/headphones.ico';
	import Paint from '$lib/components/desktop/programs/Paint.svelte';
	import Messenger from '$lib/components/desktop/programs/Messenger.svelte';
	import Media from '$lib/components/desktop/programs/Media.svelte';
	import Screensaver from '$lib/components/desktop/Screensaver.svelte';
	import { goto } from '$app/navigation';

	let time = $state(new Date());
	let showPaint = $state(false);
	let showMessenger = $state(false);
	let showMedia = $state(false);
	let showScreensaver = $state(false);

	function openPaint() {
		if (!showPaint) {
			showPaint = true;
		}
	}

	function openMessenger() {
		if (!showMessenger) {
			showMessenger = true;
		}
	}

	function openMedia() {
		if (!showMedia) {
			showMedia = true;
		}
	}

	function toggleScreensaver() {
		goto('/screensaver');
	}

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
</script>

<div class="relative flex max-h-screen min-h-screen w-full flex-col bg-[#1d80ce]">
	<!-- Program windows layer -->
	<div class="pointer-events-none absolute inset-0 z-10">
		{#if showPaint}
			<div class="pointer-events-auto">
				<Paint onClose={() => (showPaint = false)} />
			</div>
		{/if}

		{#if showMessenger}
			<div class="pointer-events-auto">
				<Messenger onClose={() => (showMessenger = false)} />
			</div>
		{/if}

		{#if showMedia}
			<div class="pointer-events-auto">
				<Media onClose={() => (showMedia = false)} />
			</div>
		{/if}
	</div>

	<!-- Desktop icons layer -->
	<div class="relative z-0 flex-grow">
		<div class="flex flex-col gap-2 p-4">
			<DesktopIcon icon={messengerIcon} label="Messenger" onClick={openMessenger} />
			<DesktopIcon icon={paintIcon} label="Paint" onClick={openPaint} />
			<DesktopIcon icon={mediaIcon} label="Media" onClick={openMedia} />
		</div>
	</div>

	<!-- Taskbar -->
	<div
		class="flex h-[40px] w-full items-center justify-between bg-gradient-to-b from-[#255c8f] to-[#214c75] px-2 shadow-lg"
	>
		<p class="font-departure text-xs text-white tracking-widest">MAYA.COMPUTER</p>
		<div class="flex gap-1">
			{#if showPaint}
				<TaskbarButton icon={paintIcon} title="Paint" isActive={true} onClick={openPaint} />
			{/if}

			{#if showMessenger}
				<TaskbarButton
					icon={messengerIcon}
					title="Messenger"
					isActive={true}
					onClick={openMessenger}
				/>
			{/if}

			{#if showMedia}
				<TaskbarButton icon={mediaIcon} title="Media" isActive={true} onClick={openMedia} />
			{/if}
		</div>

		<div class="flex items-center gap-2">
			<button
				onclick={toggleScreensaver}
				class="text-shadow flex items-center px-2 text-white hover:bg-white/10"
			>
				<span class="font-departure text-[11px] tracking-widest">SCREENSAVER</span>
			</button>

			<!-- Clock -->
			<div class="text-shadow flex items-center px-2 text-white">
				{formattedTime}
			</div>
		</div>
	</div>
</div>

<style>
	.text-shadow {
		text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.5);
	}
</style>
