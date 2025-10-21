<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    // Props
    export let href = '';
    export let text = '';
    export let target: '_blank' | '_self' | '_parent' | '_top' = '_self';
    export let rel = '';
    export let variant: 'primary' | 'secondary' | 'ghost' = 'primary';
    export let size: 'sm' | 'md' | 'lg' = 'md';
    export let disabled = false;
    export let className = '';
    export let download = false;
    export let children: any = undefined;

    // Event handlers
    function handleClick(event: MouseEvent) {
        if (disabled) {
            event.preventDefault();
            return;
        }
        dispatch('click', { event, href, text });
    }

    function handleMouseEnter(event: MouseEvent) {
        if (disabled) return;
        dispatch('mouseenter', { event, href, text });
    }

    function handleMouseLeave(event: MouseEvent) {
        if (disabled) return;
        dispatch('mouseleave', { event, href, text });
    }

    function handleFocus(event: FocusEvent) {
        if (disabled) return;
        dispatch('focus', { event, href, text });
    }

    function handleBlur(event: FocusEvent) {
        if (disabled) return;
        dispatch('blur', { event, href, text });
    }

    function handleKeydown(event: KeyboardEvent) {
        if (disabled) return;
        dispatch('keydown', { event });
    }

    // Dynamic classes based on props
    $: variantClasses = {
        primary: 'text-white hover:text-gray-300 decoration-2 decoration-dotted underline hover:underline hover:underline-offset-4',
        secondary: 'text-gray-400 hover:text-white decoration-1 decoration-solid underline hover:underline hover:underline-offset-2',
        ghost: 'text-gray-500 hover:text-white no-underline hover:underline hover:underline-offset-4'
    };

    $: sizeClasses = {
        sm: 'text-sm',
        md: 'text-base',
        lg: 'text-lg'
    };

    $: disabledClasses = disabled ? 'opacity-50 cursor-not-allowed' : 'hover:cursor-pointer transition-all duration-200';
    $: focusClasses = disabled ? '' : 'focus:outline-none focus:rounded-sm';
</script>

<a 
    {href}
    {target}
    {rel}
    {download}
    class="
    inline-block
    {variantClasses[variant]}
    {sizeClasses[size]}
    {disabledClasses}
    {focusClasses}
    {className}
    "
    on:click={handleClick}
    on:mouseenter={handleMouseEnter}
    on:mouseleave={handleMouseLeave}
    on:focus={handleFocus}
    on:blur={handleBlur}
    on:keydown={handleKeydown}
    aria-disabled={disabled}
>
    {#if children}
        {@render children?.()}
    {:else}
        {text}
    {/if}
</a>