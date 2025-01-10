<script lang="ts">
	import { fade } from 'svelte/transition';

	const { onClose } = $props();

	let video: HTMLVideoElement;
</script>

<div
	class="fixed left-0 top-0 z-[10000] cursor-none overflow-hidden bg-black"
	onclick={onClose}
	onkeyup={onClose}
	role="button"
	tabindex="0"
	aria-label="Screensaver"
	in:fade
>
	<video
		bind:this={video}
		autoplay
		loop
		class="-ml-8 h-screen min-w-[calc(100vw+20px)] object-cover"
	>
		<source
			src="https://firebasestorage.googleapis.com/v0/b/mayari-io.appspot.com/o/maya-computer%2Fpipes2.mp4?alt=media&token=2fabdc50-bc61-496c-a39e-826cb59c1811"
			type="video/mp4"
		/>
		<track kind="captions" />
	</video>
</div>
