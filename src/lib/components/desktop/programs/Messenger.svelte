<script lang="ts">
	import Window from '../Window.svelte';
	import Nudge from '$lib/assets/icons/nudge.png';

	const { onClose } = $props();
	
	// Add audio element reference
	let nudgeAudio: HTMLAudioElement;

	// Function to play the sound
	function playNudgeSound() {
		nudgeAudio.currentTime = 0; // Reset audio to start
		nudgeAudio.play();
	}
</script>

<div class="win7-elements">
	<Window
		title="Messenger"
		initialX={80}
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
				<button class="no-win7 w-fit bg-indigo-600 p-2" onclick={playNudgeSound}>
					<img src={Nudge} alt="Messenger" />
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