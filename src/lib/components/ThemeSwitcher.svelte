<script>
  import { onMount } from 'svelte';

  let current = 'light';

  // читаем стартовое состояние уже на клиенте
  onMount(() => {
    current = document.documentElement.classList.contains('dark')
      ? 'dark'
      : (localStorage.getItem('theme') ?? 'light');
    apply(current);
  });

  function apply(theme) {
    const root = document.documentElement;
    if (theme === 'dark') {
      root.classList.add('dark');
    } else {
      root.classList.remove('dark');
    }
    localStorage.setItem('theme', theme);
    current = theme;
  }

  function toggle() {
    apply(current === 'dark' ? 'light' : 'dark');
  }
</script>

<button
  on:click={toggle}
  aria-label="Toggle color theme"
  style="
    display:inline-flex;align-items:center;gap:.5rem;
    padding:.5rem .75rem;border:1px solid var(--border);
    background: var(--card); color: var(--fg); border-radius: .5rem;
    cursor:pointer;"
>
  {#if current === 'dark'}
    🌙 <span>Dark</span>
  {:else}
    ☀️ <span>Light</span>
  {/if}
</button>
