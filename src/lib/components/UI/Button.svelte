<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    // Props
    export let text = 'Click me';
    export let disabled = false;
    export let type: 'button' | 'submit' | 'reset' = 'button';
    export let variant: 'primary' | 'secondary' | 'danger' = 'primary';
    export let size: 'sm' | 'md' | 'lg' = 'md';
    export let loading = false;
    export let className = '';
    export let children: any = undefined;

    // Event handlers
    function handleClick(event: MouseEvent) {
        if (disabled || loading) return;
        dispatch('click', { event, text });
    }

    function handleMouseEnter(event: MouseEvent) {
        if (disabled || loading) return;
        dispatch('mouseenter', { event });
    }

    function handleMouseLeave(event: MouseEvent) {
        if (disabled || loading) return;
        dispatch('mouseleave', { event });
    }

    function handleFocus(event: FocusEvent) {
        if (disabled || loading) return;
        dispatch('focus', { event });
    }

    function handleBlur(event: FocusEvent) {
        if (disabled || loading) return;
        dispatch('blur', { event });
    }

    function handleKeydown(event: KeyboardEvent) {
        if (disabled || loading) return;
        dispatch('keydown', { event });
        
        // Handle Enter and Space for accessibility
        if (event.key === 'Enter' || event.key === ' ') {
            event.preventDefault();
            handleClick(event as any);
        }
    }

    // Dynamic classes based on props
    $: variantClasses = {
        primary: 'bg-black text-white border-white hover:shadow-[6px_-6px_0_0_white] active:bg-white active:text-black active:border-black active:shadow-[3px_-3px_0_0_black]',
        secondary: 'bg-white text-black border-black hover:shadow-[6px_-6px_0_0_black] active:bg-black active:text-white active:border-white active:shadow-[3px_-3px_0_0_white]',
        danger: 'bg-red-600 text-white border-red-400 hover:shadow-[6px_-6px_0_0_red-400] active:bg-red-700 active:border-red-500 active:shadow-[3px_-3px_0_0_red-500]'
    };

    $: sizeClasses = {
        sm: 'px-2 py-1 text-sm',
        md: 'px-4 py-2 text-base',
        lg: 'px-6 py-3 text-lg'
    };

    $: disabledClasses = disabled || loading ? 'opacity-50 cursor-not-allowed' : 'hover:cursor-pointer';
</script>

<button
    {type}
    {disabled}
    class="
    border-3
    transition-all 
    duration-300 
    ease-in-out 
    active:translate-x-[3px]
    active:translate-y-[-3px]
    active:scale-95
    {variantClasses[variant]}
    {sizeClasses[size]}
    {disabledClasses}
    {className}
    "
    on:click={handleClick}
    on:mouseenter={handleMouseEnter}
    on:mouseleave={handleMouseLeave}
    on:focus={handleFocus}
    on:blur={handleBlur}
    on:keydown={handleKeydown}
>
    {#if loading}
        <span class="inline-flex items-center">
            <svg class="animate-spin -ml-1 mr-2 h-4 w-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
            Loading...
        </span>
    {:else}
        {#if children}
            {@render children?.()}
        {:else}
            {text}
        {/if}
    {/if}
</button>