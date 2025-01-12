<script lang="ts">
	import { fade } from 'svelte/transition';

	const {
		title = 'Window',
		maxWidth = '300px',
		maxHeight = '500px',
		minWidth = '280px',
		children,
		initialX = 50,
		initialY = 50,
		zIndex = 0,
		onClose,
		hasSpace = false
	} = $props();

	let isDragging = $state(false);
	let isMaximized = $state(false);
	let position = $state({ x: initialX, y: initialY });
	let offset = $state({ x: 0, y: 0 });
	let windowRef = $state<HTMLDivElement | null>(null);
	let currentZIndex = $state(zIndex);
	let isShaking = $state(false);

	function handleMouseDown(event: MouseEvent) {
		isDragging = true;
		offset.x = event.clientX - position.x;
		offset.y = event.clientY - position.y;
		// Bring window to front
		currentZIndex = getHighestZIndex() + 1;
	}

	function handleMouseMove(event: MouseEvent) {
		if (!isDragging) return;

		// Calculate new position
		let newX = event.clientX - offset.x;
		let newY = event.clientY - offset.y;

		position.x = newX;
		position.y = newY;
	}

	function handleMouseUp() {
		isDragging = false;
	}

	function getHighestZIndex(): number {
		const windows = document.querySelectorAll('.window');
		let maxZ = 0;
		windows.forEach((win) => {
			const z = parseInt(getComputedStyle(win).zIndex) || 0;
			maxZ = Math.max(maxZ, z);
		});
		return maxZ;
	}

	export function shake() {
		if (isShaking) return;
		isShaking = true;
		setTimeout(() => {
			isShaking = false;
		}, 1000);
	}

	$effect(() => {
		if (isDragging) {
			window.addEventListener('mousemove', handleMouseMove);
			window.addEventListener('mouseup', handleMouseUp);
		}

		return () => {
			window.removeEventListener('mousemove', handleMouseMove);
			window.removeEventListener('mouseup', handleMouseUp);
		};
	});
</script>

<div
	bind:this={windowRef}
	class="window active glass"
	class:shaking={isShaking}
	transition:fade={{ duration: 150 }}
	style="
      max-width: {isMaximized ? '100%' : maxWidth}; 
      max-height: {isMaximized ? '100%' : maxHeight};
      min-width: {minWidth};
      width: {isMaximized ? '100%' : 'clamp(' + minWidth + ', 90vw, ' + maxWidth + ')'};
      height: {isMaximized ? '100%' : 'auto'};
      transform: translate({position.x}px, {position.y}px);
      z-index: {currentZIndex};
      --window-background-color: #805ba5;
    "
>
	<div class="title-bar cursor-move" onmousedown={handleMouseDown} role="toolbar" tabindex="0">
		<div class="title-bar-text select-none">{title}</div>
		<div class="title-bar-controls">
			<button aria-label="Close" onclick={onClose}></button>
		</div>
	</div>
	<div class="window-body" class:has-space={hasSpace} style="overflow-y: auto;">
		{@render children()}
	</div>
</div>

<style lang="postcss">
	.shaking {
		animation: shake 1s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
	}

	@keyframes shake {
		0%,
		100% {
			transform: translateX(0);
		}
		20% {
			transform: translateX(-10px);
		}
		40% {
			transform: translateX(10px);
		}
		60% {
			transform: translateX(-10px);
		}
		80% {
			transform: translateX(10px);
		}
	}
</style>
