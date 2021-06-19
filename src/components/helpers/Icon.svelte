<script>
  import feather from "feather-icons";
  export const directions = ["n", "ne", "e", "se", "s", "sw", "w", "nw"];

  export let name;
  export let direction = "n";
  export let strokeWidth;
  export let stroke;
  export let width = "1em";
  export let height = "1em";

  $: icon = feather.icons[name];
  $: rotation = directions.indexOf(direction) * 45;
  $: if (icon) {
    if (stroke) icon.attrs["stroke"] = stroke;
    if (strokeWidth) icon.attrs["stroke-width"] = strokeWidth;
  }
</script>

{#if icon}
  <svg
    {...icon.attrs}
    style="width: {width}; height: {height}; transform: translate(0.125rem, 0.125rem) rotate({rotation}deg);"
  >
    <g>
      {@html icon.contents}
    </g>
  </svg>
{/if}

<style>
  svg {
    width: 1em;
    height: 1em;
    transform: translateY(0.125rem);
    overflow: visible;
    transform-origin: 50% 50%;
    fill: #282828;
  }
</style>
