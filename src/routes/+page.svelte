<script lang="ts">
  import { base } from '$app/paths';
  import HomeSlider from '$lib/components/HomeSlider.svelte';
</script>

<section class="d-flex flex-column align-items-center" id="home">
  <p>mRemoteNG (Multi-Remote Next Generation), abbreviated as mR, is an open-source, tabbed remote connections manager for Windows.</p>
  <p>It allows you to view all of your remote connections in a simple yet powerful interface and supports multiple protocols including RDP, VNC, SSH, HTTP/S and more.</p>

  <!-- ширина как у прежней картинки, при желании скорректируй -->
  <div class="w-100" style="max-width: 1024px;">
    <HomeSlider count={5} dir="images/slider" height={460} />
  </div>
</section>
