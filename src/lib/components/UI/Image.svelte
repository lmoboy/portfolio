<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    // Props
    export let src = '';
    export let alt = '';
    export let clickable = false;
    export let className = '';
    export let loading: 'eager' | 'lazy' = 'lazy';
    export let draggable = false;
    export let objectFit: 'cover' | 'contain' | 'fill' | 'none' | 'scale-down' = 'cover';
    export let aspectRatio = 'auto';
    export let children: any = undefined;

    // State
    let imageLoaded = false;
    let imageError = false;

    // Event handlers
    function handleClick(event: MouseEvent) {
        if (!clickable) return;
        dispatch('click', { event, src, alt });
    }

    function handleMouseEnter(event: MouseEvent) {
        dispatch('mouseenter', { event, src, alt });
    }

    function handleMouseLeave(event: MouseEvent) {
        dispatch('mouseleave', { event, src, alt });
    }

    function handleLoad(event: Event) {
        imageLoaded = true;
        imageError = false;
        dispatch('load', { event, src, alt });
    }

    function handleError(event: Event) {
        imageLoaded = false;
        imageError = true;
        dispatch('error', { event, src, alt });
    }

    function handleKeydown(event: KeyboardEvent) {
        if (!clickable) return;
        dispatch('keydown', { event });
        
        // Handle Enter and Space for accessibility
        if (event.key === 'Enter' || event.key === ' ') {
            event.preventDefault();
            handleClick(event as any);
        }
    }

    // Dynamic classes
    $: clickableClasses = clickable ? 'hover:shadow-[6px_-6px_0_0_white] hover:cursor-pointer active:translate-x-[3px] active:translate-y-[-3px] active:shadow-[3px_-3px_0_0_white] active:scale-95 focus:outline-none ' : '';
    $: objectFitClass = `object-${objectFit}`;
    $: aspectRatioStyle = aspectRatio !== 'auto' ? `aspect-ratio: ${aspectRatio};` : '';
</script>

<div 
    class="
    bg-black 
    border-3
    border-white 
    overflow-hidden
    transition-all 
    duration-300 
    ease-in-out 
    {clickableClasses}
    {className}
    "
    style={aspectRatioStyle}
    role={clickable ? 'button' : 'img'}
    tabindex={clickable ? 0 : undefined}
    onclick={handleClick}
    onmouseenter={handleMouseEnter}
    onmouseleave={handleMouseLeave}
    onkeydown={handleKeydown}
>
    {#if imageError}
        <div class="w-full h-full flex items-center justify-center bg-gray-800 text-gray-400">
            <div class="text-center">
                <svg class="w-12 h-12 mx-auto mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                </svg>
                <p class="text-sm">Failed to load image</p>
            </div>
        </div>
    {:else}
        <img 
            {src} 
            {alt} 
            loading={loading}
            {draggable}
            class="w-full h-full {objectFitClass} transition-opacity duration-300 {imageLoaded ? 'opacity-100' : 'opacity-0'}"
            onload={handleLoad} 
            onerror={handleError}
        />
        {#if !imageLoaded && !imageError}
            <div class="absolute inset-0 w-full h-full flex items-center justify-center bg-gray-800">
                <svg class="animate-spin h-8 w-8 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
            </div>
        {/if}
    {/if}
    
    {#if children}
        <div class="absolute inset-0 pointer-events-none">
            {@render children?.()}
        </div>
    {/if}
</div>

