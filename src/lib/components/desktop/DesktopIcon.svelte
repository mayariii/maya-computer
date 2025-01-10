<script lang="ts">
  export let icon: string;
  export let label: string;
  export let onClick: () => void;
</script>

<button
  on:click={onClick}
  class="flex flex-col items-center w-[75px] p-2 rounded  focus:bg-white/20"
>
  <img
    src={icon}
    alt={label}
    class="w-8 h-8 mb-1"
  />
  <span class="text-white text-sm text-center break-words text-shadow">
    {label}
  </span>
</button>

<style>
  .text-shadow {
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
  }
</style> 