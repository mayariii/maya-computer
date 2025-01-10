<script lang="ts">
	import Window from '../Window.svelte';
	import Nudge from '$lib/assets/icons/nudge.png';

	const { onClose } = $props();

	let nudgeAudio: HTMLAudioElement;
	let isShaking = $state(false);

	function playNudgeSound() {
		nudgeAudio.currentTime = 0;
		nudgeAudio.play();
		isShaking = true;

		// Reset shaking after animation completes
		setTimeout(() => {
			isShaking = false;
		}, 500);
	}
</script>

<div class="win7-elements" class:shake={isShaking}>
	<Window
		title="Messenger"
		initialX={20}
		initialY={10}
		maxWidth="900px"
		maxHeight="1200px"
		{onClose}
	>
		<div class="relative w-full">
			<iframe
				title="Messenger"
				src="https://www5.cbox.ws/box/?boxid=954883&boxtag=yqaHcZ"
				width="100%"
				height="450"
				allow="autoplay"
				frameborder="0"
				marginheight="0"
				marginwidth="0"
				scrolling="auto"
			></iframe>

			<div class="bg-violet-100 p-2">
				<button class="no-win7 flex items-center gap-2 bg-indigo-600 p-4" onclick={playNudgeSound}>
					<img src={Nudge} alt="Messenger" class="w-4 h-4" />
					<span class="font-departure text-xs">Nudge</span>
				</button>
			</div>
		</div>
	</Window>
</div>

<audio
	bind:this={nudgeAudio}
	src="https://firebasestorage.googleapis.com/v0/b/mayari-io.appspot.com/o/maya-computer%2Fnudge.mp3?alt=media&token=fb55e0be-bde9-4372-b6e4-0ae8cee4aab4"
	preload="auto"
></audio>

<style>
	.shake {
		animation: shake 0.5s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
		transform: translate3d(0, 0, 0);
	}

	@keyframes shake {
		10%,
		90% {
			transform: translate3d(-3px, 0, 0);
		}

		20%,
		80% {
			transform: translate3d(6px, 0, 0);
		}

		30%,
		50%,
		70% {
			transform: translate3d(-12px, 0, 0);
		}

		40%,
		60% {
			transform: translate3d(12px, 0, 0);
		}
	}
</style>
