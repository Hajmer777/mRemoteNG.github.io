<script lang="ts">
  import { onMount } from 'svelte';
  import { browser } from '$app/environment';
  import { currentLocale, setLocale, availableLocales, languageNames } from '$lib/i18n/store';

  let open = false;
  let root: HTMLElement;

  function toggle() {
    open = !open;
  }
  function pick(loc: string) {
    setLocale(loc);
    open = false;
  }

  function onDocClick(e: MouseEvent) {
    if (root && !root.contains(e.target as Node)) open = false;
  }
  function onKey(e: KeyboardEvent) {
    if (e.key === 'Escape') open = false;
  }

  onMount(() => {
    if (!browser) return;
    document.addEventListener('click', onDocClick, true);
    document.addEventListener('keydown', onKey);
    return () => {
      document.removeEventListener('click', onDocClick, true);
      document.removeEventListener('keydown', onKey);
    };
  });
</script>

<li class="nav-item dropdown" bind:this={root} data-bs-theme="light">
  <button
    type="button"
    id="lang-menu"
    class="nav-link dropdown-toggle d-flex align-items-center"
    aria-haspopup="true"
    aria-expanded={open}
    on:click={toggle}
  >
    <span class="lang-text">{$languageNames[$currentLocale] ?? $currentLocale.toUpperCase()}</span>
  </button>

  <ul class="dropdown-menu dropdown-menu-end {open ? 'show' : ''}" aria-labelledby="lang-menu">
    {#each availableLocales as localeCode (localeCode)}
      <li style="width: 150px;">
        <button
          type="button"
          class="dropdown-item { $currentLocale === localeCode ? 'active' : '' }"
          on:click={() => pick(localeCode)}
          role="menuitem"
          style="width: 100%; text-align: left;"
        >
          {$languageNames[localeCode] ?? localeCode.toUpperCase()}
        </button>
      </li>
    {/each}
  </ul>
</li>

<style>
  /* чтобы точно было видно меню поверх контента */
  .dropdown-menu { z-index: 1200; min-width: 10rem; }
</style>
