<script>
  import { goto } from "$app/navigation";
</script>

<div
  class="grid grid-cols-2 grid-rows-1 w-full h-[100vh] justify-around items-center"
>
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
</div>
