<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  // Types
  interface QueueItem {
    from: string;
    to: string;
    start: number;
    end: number;
    char: string;
  }

  // Props
  export let text = '';
  export let chars = '!<>-_\\/[]{}@€"=+*^?#________';
  export let speed = 1; // Animation speed multiplier (higher = faster)
  export let scrambleRange = 40; // Range for scramble timing
  export let charChangeChance = 0.28; // Probability of character change per frame
  export let autoStart = true; // Start animation automatically
  export let className = ''; // For custom styling

  // State
  let spanElement: HTMLSpanElement;
  let output = '';
  let queue: QueueItem[] = [];
  let frame = 0;
  let frameRequest: number | undefined;
  let currentText = '';

  // Generate random character from chars string
  function randomChar() {
    return chars[Math.floor(Math.random() * chars.length)];
  }

  // Update animation frame
  function update() {
    // Check if we're in browser environment
    if (typeof window === 'undefined') {
      return;
    }

    let newOutput = '';
    let complete = 0;

    for (let i = 0; i < queue.length; i++) {
      let { from, to, start, end } = queue[i];

      if (frame >= end) {
        complete++;
        newOutput += to;
      } else if (frame >= start) {
        if (!queue[i].char || Math.random() < charChangeChance) {
          queue[i].char = randomChar();
        }
        newOutput += `<span class="scramble-char">${queue[i].char}</span>`;
      } else {
        newOutput += from;
      }
    }

    output = newOutput;

    if (complete === queue.length) {
      currentText = text;
      // Animation complete
    } else {
      frameRequest = requestAnimationFrame(update);
      frame += speed;
    }
  }

  // Start scramble animation
  function scramble(newText: string) {
    // Check if we're in browser environment
    if (typeof window === 'undefined') {
      output = newText;
      currentText = newText;
      return;
    }

    const oldText = currentText;
    const length = Math.max(oldText.length, newText.length);
    
    queue = [];
    
    for (let i = 0; i < length; i++) {
      const from = oldText[i] || '';
      const to = newText[i] || '';
      const start = Math.floor(Math.random() * scrambleRange);
      const end = start + Math.floor(Math.random() * scrambleRange);
      queue.push({ from, to, start, end, char: '' });
    }

    if (frameRequest) {
      cancelAnimationFrame(frameRequest);
    }

    frame = 0;
    update();
  }

  // Public method to trigger scramble manually
  export function trigger(newText: string = text) {
    scramble(newText);
  }

  // Watch for text changes
  $: if (text !== currentText && autoStart && typeof window !== 'undefined') {
    scramble(text);
  }

  onMount(() => {
    if (typeof window !== 'undefined') {
      if (autoStart && text) {
        scramble(text);
      } else {
        output = text;
        currentText = text;
      }
    } else {
      output = text;
      currentText = text;
    }
  });

  onDestroy(() => {
    if (typeof window !== 'undefined' && frameRequest) {
      cancelAnimationFrame(frameRequest);
    }
  });
</script>

<span class={className} bind:this={spanElement}>
  {@html output}
</span>

<style>
  :global(.scramble-char) {
    opacity: 0.5;
  }
</style>

