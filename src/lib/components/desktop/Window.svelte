<script lang="ts">
  import { fade } from 'svelte/transition';
  
  const { 
    title = "Window", 
    maxWidth = "300px", 
    maxHeight = "500px",
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
  
  let preMaximizeState = $state({
    x: initialX,
    y: initialY,
    width: '',
    height: ''
  });

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
    
    // Get window and desktop dimensions
    const windowRect = windowRef?.getBoundingClientRect();
    const desktopRect = document.querySelector('.desktop-area')?.getBoundingClientRect();
    
    if (windowRect && desktopRect) {
      // Bound checking
      newX = Math.max(-20, Math.min(newX, desktopRect.width - windowRect.width + 20));
      newY = Math.max(0, Math.min(newY, desktopRect.height - windowRect.height));
    }
    
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

  function handleMaximize() {
    if (!isMaximized) {
      // Store current state before maximizing
      const rect = windowRef?.getBoundingClientRect();
      preMaximizeState = {
        x: position.x,
        y: position.y,
        width: windowRef?.style.width || '',
        height: windowRef?.style.height || ''
      };
      
      // Maximize
      position.x = 0;
      position.y = 0;
      isMaximized = true;
    } else {
      // Restore previous state
      position.x = preMaximizeState.x;
      position.y = preMaximizeState.y;
      if (windowRef) {
        windowRef.style.width = preMaximizeState.width;
        windowRef.style.height = preMaximizeState.height;
      }
      isMaximized = false;
    }
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
		class="window active absolute glass"
		transition:fade={{ duration: 150 }}
		style="
    max-width: {isMaximized ? '100%' : maxWidth}; 
    max-height: {isMaximized ? '100%' : maxHeight};
    width: {isMaximized ? '100%' : 'auto'};
    height: {isMaximized ? '100%' : 'auto'};
    transform: translate({position.x}px, {position.y}px);
    z-index: {currentZIndex};
    --window-background-color: #805ba5;
  "
	>
		<div class="title-bar cursor-move" onmousedown={handleMouseDown} role="toolbar" tabindex="0">
			<div class="title-bar-text select-none">{title}</div>
			<div class="title-bar-controls">
				<!-- <button aria-label="Minimize"></button> -->
				<button aria-label="Maximize" onclick={handleMaximize}></button>
				<button aria-label="Close" onclick={onClose}></button>
			</div>
		</div>
		<div class="window-body" class:has-space={hasSpace} style="overflow-y: auto;">
			{@render children()}
		</div>
	</div>
