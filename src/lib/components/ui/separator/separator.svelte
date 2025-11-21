<script>
  import { cn } from "$lib/utils.js";

  let className = undefined;
  export { className as class };
  export let orientation = "horizontal";
  export let decorative = undefined;
</script>

<div
  role={decorative ? "none" : "separator"}
  aria-orientation={orientation}
  class={cn(
    "shrink-0 bg-border",
    orientation === "horizontal" ? "h-[1px] w-full" : "h-full w-[1px]",
    className
  )}
  {...$$restProps}
></div>
