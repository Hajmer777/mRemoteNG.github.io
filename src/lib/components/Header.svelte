<script lang="ts">
  import { page } from '$app/stores';
  import { base } from '$app/paths';
  import ThemeSwitcher from './ThemeSwitcher.svelte';
  import LanguageSwitcher from './LanguageSwitcher.svelte';
  import { t } from '$lib/i18n/store';

  const menuItems = [
    { titleKey: 'header.about',       path: '/' },
    { titleKey: 'header.feed',        path: '/feed' },
    { titleKey: 'header.contribute',  path: '/contribute' },
    { titleKey: 'header.contact',     path: '/contact' }
  ];

  let isDropdownOpen = false;

  const socialLinks = [
    { name: 'Twitter',  icon: 'bi bi-twitter',   url: 'https://x.com/mremoteng',     tooltip: 'X (Twitter)' },
    { name: 'Telegram', icon: 'bi bi-telegram',  url: 'https://t.me/yourchannel',    tooltip: 'Telegram' },
    { name: 'Reddit',   icon: 'bi bi-reddit',    url: 'https://reddit.com/yourpage', tooltip: 'Reddit' }
  ];

  const toggleDropdown = () => { isDropdownOpen = !isDropdownOpen; };
</script>

<header>
  <nav class="navbar navbar-expand-lg site-header fixed-top">
    <div class="container">
      <a href={base + '/'} class="navbar-brand">mRemoteNG</a>

      <!-- Кнопка-бургер; у иконки своя aria-label у самой кнопки -->
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarResponsive"
        aria-controls="navbarResponsive"
        aria-expanded="false"
        aria-label="Toggle navigation">
        <i class="bi bi-list" aria-hidden="true"></i>
      </button>

      <div class="collapse navbar-collapse" id="navbarResponsive">
        <!-- Левое меню -->
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          {#each menuItems as item}
            <li class="nav-item">
              <a
                class="nav-link"
                href={base + item.path}
                aria-current={$page.url.pathname === (base + item.path).replace(/\/$/, '') ? 'page' : undefined}>
                {$t(item.titleKey)}
              </a>
            </li>
          {/each}
        </ul>

        <!-- Правый блок -->
        <ul class="navbar-nav ms-md-auto align-items-center gap-1">
          <li class="nav-item">
            <a rel="noopener" class="nav-link" href={base + '/downloads'} title="Downloads">
              <i class="bi bi-download" aria-hidden="true"></i>
              <span class="d-lg-none ms-2">{$t('header.downloads')}</span>
            </a>
          </li>

          <li class="nav-item">
            <a target="_blank" rel="noopener" class="nav-link" href="https://github.com/mRemoteNG/mRemoteNG" title="GitHub">
              <i class="bi bi-github" aria-hidden="true"></i>
              <span class="d-lg-none ms-2">{$t('header.github')}</span>
            </a>
          </li>

          <!-- Соц. ссылки: кнопка с aria-label, дропдаун управляем сами -->
          <li class="nav-item dropdown position-static">
            <button
              class="nav-link dropdown-toggle"
              type="button"
              title="Social links"
              aria-label="Open social links"
              aria-expanded={isDropdownOpen}
              on:click={toggleDropdown}>
              <i class="bi bi-share" aria-hidden="true"></i>
            </button>

            <ul class={"dropdown-menu dropdown-menu-end" + (isDropdownOpen ? " show" : "")} style="right: 0; left: auto;">
              {#each socialLinks as link}
                <li>
                  <a
                    class="dropdown-item"
                    target="_blank"
                    rel="noopener"
                    title={link.tooltip}
                    href={link.url}>
                    <i class={link.icon} aria-hidden="true"></i>
                    <span class="ms-2">{link.name}</span>
                  </a>
                </li>
              {/each}
            </ul>
          </li>

          <!-- разделитель -->
          <li class="nav-item py-2 py-lg-1 col-12 col-lg-auto d-none d-lg-block">
            <div class="vr mx-lg-2"></div>
          </li>

          <!-- LanguageSwitcher САМ рендерит <li>, поэтому без обёртки -->
          <LanguageSwitcher />

          <!-- разделитель -->
          <li class="nav-item py-2 py-lg-1 col-12 col-lg-auto d-none d-lg-block">
            <div class="vr mx-lg-2"></div>
          </li>

          <!-- ThemeSwitcher (если он не <li>, ок — браузер чинит; при желании обернуть в <li class="nav-item">) -->
          <ThemeSwitcher />
        </ul>
      </div>
    </div>
  </nav>
</header>

<style>
  /* цвета и фон берутся из app.css по .site-header */
  header { position: relative; z-index: 1030; }

  .site-header .navbar-toggler {
    border: 1px solid var(--header-border);
  }
  .site-header .navbar-toggler .bi {
    font-size: 1.25rem;
    color: var(--header-fg);
    line-height: 1;
    display: inline-block;
  }

  .site-header .vr {
    opacity: 1;
    border-color: var(--header-border);
  }

  .dropdown-menu { min-width: 12rem; }
</style>
