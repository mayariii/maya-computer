<script lang="ts">
	import Window from '../Window.svelte';
	import player from '$lib/assets/images/player.png';

	const { onClose } = $props();

	let audio: HTMLAudioElement;
	let video: HTMLVideoElement;
	let isPlaying = $state(false);

	function togglePlay() {
		if (isPlaying) {
			audio.pause();
			video.pause();
		} else {
			audio.play();
			video.play();
		}
		isPlaying = !isPlaying;
	}

	function stopAudio() {
		audio.pause();
		video.pause();
		audio.currentTime = 0;
		video.currentTime = 0;
		isPlaying = false;
	}
</script>

<div class="win7-elements">
	<Window
		title="Media Player"
		initialX={20}
		initialY={10}
		maxWidth="500px"
		maxHeight="1200px"
		{onClose}
	>
		<div class="relative flex flex-col items-center justify-center bg-black p-2">
			<div
				class="text-whit absolute left-1/2 top-[90px] z-20 mb-2 w-[220px] -translate-x-1/2 overflow-hidden"
			>
				<p class="animate-marquee whitespace-nowrap font-departure">
					Castles in the Sky [Limewire].mp3
				</p>
			</div>
			<div class="mb-4 flex w-full justify-center gap-2">
				<audio
					bind:this={audio}
					src="https://firebasestorage.googleapis.com/v0/b/mayari-io.appspot.com/o/maya-computer%2FCastles%20In%20The%20Sky%20Radio%20Mix.mp3?alt=media&token=2ff6710c-be92-4f3f-a642-7b1a3bd66f5c"
				></audio>
				<button class="rounded bg-green-500 px-4 py-2 hover:bg-green-600" onclick={togglePlay}>
					{isPlaying ? 'Pause' : 'Play'}
				</button>
				<button class="rounded bg-red-500 px-4 py-2 hover:bg-red-600" onclick={stopAudio}>
					Stop
				</button>
			</div>
			<video
				bind:this={video}
				loop
				class="absolute inset-y-0 top-[130px] h-[240px] w-[230px] rounded-[80px] sm:h-[200px] sm:w-fit sm:rounded-[50px]"
			>
				<source
					src="https://firebasestorage.googleapis.com/v0/b/mayari-io.appspot.com/o/maya-computer%2FScreen%20Recording%202025-01-10%20at%2021.44.44.mov?alt=media&token=64bdc29a-9508-420a-85ad-b917985ce15a"
					type="video/mp4"
				/>
				<track kind="captions" />
			</video>
			<img src={player} alt="Player" class="relative z-10 h-[500px] w-fit" />
		</div>
	</Window>
</div>

<style>
	.animate-marquee {
		animation: marquee 10s linear infinite;
	}

	@keyframes marquee {
		0% {
			transform: translateX(100%);
		}
		100% {
			transform: translateX(-100%);
		}
	}
</style>
