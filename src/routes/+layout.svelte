<script lang="ts">
  import { goto } from "$app/navigation";
  import "../app.css";

  let { children } = $props();
</script>

<header>
  <button
    class="btn bg-amber-500 h-1/2"
    onclick={() => {
      goto("skeleton");
    }}>Skeleton test</button
  >

  <button
    class="btn bg-green-500 h-1/2"
    onclick={() => {
      goto("zig");
    }}>Zig test</button
  >
</header>
{@render children()}
