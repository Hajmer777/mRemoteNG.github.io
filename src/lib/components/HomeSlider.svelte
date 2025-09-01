<script lang="ts">
  import { onMount } from 'svelte';
  import { base } from '$app/paths';

  export let count = 5;
  export let dir = 'images/slider';
  export let ext = 'webp';
  export let height = 460;
  export let autoplay = false;
  export let interval = 5000;

  const urls = Array.from({ length: count }, (_, i) => `${base}/${dir}/${i + 1}.${ext}`);

  let index = 0;
  let timer: number | null = null;

  function goTo(i: number) { index = (i + urls.length) % urls.length; }
  function next() { goTo(index + 1); }
  function prev() { goTo(index - 1); }

  function start() { if (autoplay) { stop(); timer = setInterval(next, interval) as unknown as number; } }
  function stop() { if (timer) { clearInterval(timer); timer = null; } }

  // свайпы
  let startX = 0, deltaX = 0;
  function onTouchStart(e: TouchEvent) { startX = e.touches[0].clientX; deltaX = 0; stop(); }
  function onTouchMove(e: TouchEvent) { deltaX = e.touches[0].clientX - startX; }
  function onTouchEnd() { if (Math.abs(deltaX) > 50) (deltaX < 0 ? next() : prev()); start(); }

  function onKey(e: KeyboardEvent) { if (e.key === 'ArrowLeft') prev(); if (e.key === 'ArrowRight') next(); }

  onMount(() => {
    urls.forEach(src => { const img = new Image(); img.src = src; });
    document.addEventListener('keydown', onKey);
    start();
    return () => { document.removeEventListener('keydown', onKey); stop(); };
  });
</script>

<div class="slider"
     on:touchstart={onTouchStart}
     on:touchmove={onTouchMove}
     on:touchend={onTouchEnd}>
  {#each urls as src, i}
    <img
      src={src}
      alt={`slide ${i + 1}`}
      class="slide {i === index ? 'active' : ''}"
      loading={i === 0 ? 'eager' : 'lazy'}
      style="object-fit: cover; max-height: {height}px;" />
  {/each}

  {#if urls.length > 1}
    <button class="nav prev" aria-label="Previous" on:click={prev}>&lsaquo;</button>
    <button class="nav next" aria-label="Next" on:click={next}>&rsaquo;</button>

    <div class="dots">
      {#each urls as _, i}
        <button
          class="dot"
          class:active={i === index}
          aria-label={`Go to slide ${i + 1}`}
          on:click={() => goTo(i)}>
        </button>
      {/each}
    </div>
  {/if}
</div>

<style>
  .slider { position: relative; width: 100%; overflow: hidden; border-radius: .5rem; }

  .slide {
    position: absolute; inset: 0;
    width: 100%; height: auto; object-fit: cover;
    opacity: 0; transition: opacity .25s ease-in-out;
  }
  .slide.active { opacity: 1; position: relative; }

  .nav {
    position: absolute; top: 50%; transform: translateY(-50%);
    width: 40px; height: 40px; font-size: 24px; line-height: 38px; text-align: center;
    border-radius: 999px; cursor: pointer; user-select: none;

    /* ТОЛЬКО переменные */
    background: var(--btn-bg);
    color: var(--btn-fg);
    border: 1px solid var(--btn-border);
  }
  .nav:hover { background: var(--surface-2); }
  .nav.prev { left: 8px; }
  .nav.next { right: 8px; }

  .dots { position: absolute; bottom: 8px; left: 0; right: 0; display: flex; gap: 6px; justify-content: center; }
  .dot {
    width: 10px; height: 10px; border-radius: 50%;
    background: var(--dot-bg);
    border: 1px solid var(--dot-active-br);
    cursor: pointer;
  }
  .dot.active {
    background: var(--dot-active-bg);
    border-color: var(--dot-active-br);
  }

  @media (max-width: 768px) {.slide { max-height: 300px; }}
</style>
