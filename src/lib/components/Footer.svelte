<script lang="ts">
  import { t } from '$lib/i18n/store.js';
  import { base } from '$app/paths';
  import { onMount } from 'svelte';

  const currentYear = new Date().getFullYear();
  let el: HTMLElement;

  onMount(() => {
    const setH = () => {
      if (!el) return;
      document.documentElement.style.setProperty('--footer-h', `${el.offsetHeight}px`);
    };
    setH();
    const ro = new ResizeObserver(setH);
    ro.observe(el);
    window.addEventListener('resize', setH);
    return () => {
      ro.disconnect();
      window.removeEventListener('resize', setH);
    };
  });
</script>

<footer id="main-footer" class="site-footer mt-auto py-3" bind:this={el}>
  <div class="container d-flex justify-content-between align-items-center gap-3 flex-wrap">
    <div id="copyright" class="small">
      <p class="mb-1">&copy; {currentYear} {$t('footer.rights')}</p>
      <p class="mb-0">
        mRemoteNG is open source software and is released under the terms of the
        <a href="https://www.gnu.org/licenses/old-licenses/gpl-2.0-standalone.html" target="_blank" rel="noopener noreferrer">
          GNU General Public License Version 2
        </a>.
      </p>
    </div>

    <div id="ssl-seal" class="ms-auto">
      <img
        src="{base}/images/certum_pl_dv.svg"
        alt="Certum SSL Seal"
        width="211"
        height="64"
        loading="lazy"
      />
    </div>
  </div>
</footer>
