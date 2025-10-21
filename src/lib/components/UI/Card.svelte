<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	let { 
		children, 
		className = '', 
		clickable = false, 
		interactive = false,
		hoverable = true 
	} = $props();

	// Event handlers
	function handleClick(event: MouseEvent) {
		if (!clickable && !interactive) return;
		dispatch('click', { event });
	}

	function handleMouseEnter(event: MouseEvent) {
		dispatch('mouseenter', { event });
	}

	function handleMouseLeave(event: MouseEvent) {
		dispatch('mouseleave', { event });
	}

	function handleFocus(event: FocusEvent) {
		if (!interactive) return;
		dispatch('focus', { event });
	}

	function handleBlur(event: FocusEvent) {
		if (!interactive) return;
		dispatch('blur', { event });
	}

	function handleKeydown(event: KeyboardEvent) {
		if (!interactive) return;
		dispatch('keydown', { event });
	}

	// Dynamic classes
	let interactiveClasses = $derived(clickable || interactive ? 'hover:cursor-pointer focus:outline-none' : 'hover:cursor-default');
	let hoverShadow = $derived(hoverable ? 'hover:shadow-[6px_-6px_0_0_white]' : '');
</script>

<div
	class="
		flex
		flex-col
		p-6
		border-3
		border-white
		bg-black
		transition-all 
		duration-300 
		ease-in-out
		{interactiveClasses}
		{hoverShadow}
		{className}
	"
	role={interactive ? 'button' : undefined}
	tabindex={interactive ? 0 : -1}
	onclick={handleClick}
	onmouseenter={handleMouseEnter}
	onmouseleave={handleMouseLeave}
	onfocus={handleFocus}
	onblur={handleBlur}
	onkeydown={handleKeydown}
>
	{@render children?.()}
</div>
