<script>
  export let filled = false;

  export let color;
  export let size;
  export let type;
  export let variant;

  $: path =
    type === "star"
      ? "m25,1 6,17h18l-14,11 5,17-15-10-15,10 5-17-14-11h18z"
      : "M44.395 25l-19.395 19.395-19.395-19.395a13.714 13.714 90 0 1 19.395-19.395 13.714 13.714 90 0 1 19.395 19.395z";

  $: className = filled ? variant : "outline";
</script>

<svg
  width={size}
  height={size}
  viewBox="0 0 50 50"
  class="icon"
  on:mouseover
  on:focus
  on:mouseleave
  on:click
  let:class
>
  <path class={className} d={path} style="--rating-custom-color: {color}" />
</svg>

<style>
  .icon {
    margin-right: 2px;
  }
  .outline {
    fill: none;
    stroke: var(--spectrum-global-color-gray-600);
  }

  .primary {
    fill: var(--primaryColor);
  }
  .secondary {
    fill: var(--primaryColorHover);
  }
  .mono {
    fill: var(
      --spectrum-button-primary-m-background-color-down,
      var(--spectrum-global-color-gray-900)
    );
  }
  .gold {
    fill: #ffd700;
  }

  .red {
    fill: var(--spectrum-global-color-red-400, red);
  }

  .custom {
    fill: var(--rating-custom-color)
  }
</style>
