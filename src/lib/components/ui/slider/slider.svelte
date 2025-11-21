<script>
  import { cn } from "$lib/utils.js";

  let className = undefined;
  export { className as class };
  export let value = [50];
  export let min = 0;
  export let max = 100;
  export let step = 1;

  let sliderValue = value[0];

  function handleInput(event) {
    sliderValue = Number(event.target.value);
    value = [sliderValue];
  }

  $: if (value[0] !== sliderValue) {
    sliderValue = value[0];
  }
</script>

<div class={cn("relative flex w-full touch-none select-none items-center", className)} {...$$restProps}>
  <div class="relative h-2 w-full grow overflow-hidden rounded-full bg-secondary">
    <div
      class="absolute h-full bg-primary transition-all"
      style="width: {((sliderValue - min) / (max - min)) * 100}%"
    ></div>
  </div>
  <input
    type="range"
    {min}
    {max}
    {step}
    value={sliderValue}
    on:input={handleInput}
    class="absolute w-full h-2 opacity-0 cursor-pointer"
  />
  <div
    class="absolute block h-5 w-5 rounded-full border-2 border-primary bg-background ring-offset-background transition-all focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 pointer-events-none"
    style="left: calc({((sliderValue - min) / (max - min)) * 100}% - 0.625rem)"
  ></div>
</div>
