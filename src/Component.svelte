<script>
  import { getContext, onDestroy } from "svelte";
  import Icon from "./Icon.svelte";

  export let color;
  export let field;
  export let label;

  export let customSize = 24;
  export let numberOfStars = 5;
  export let size = "24px";
  export let type = "star";
  export let validation;
  export let variant = "mono";

  const { styleable } = getContext("sdk");
  const component = getContext("component");
  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const fieldGroupContext = getContext("field-group");

  let fieldApi;
  let fieldState;
  let hoverRating = 0;

  const formApi = formContext?.formApi;
  const labelPos = fieldGroupContext?.labelPosition || "above";
  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: formField = formApi?.registerField(
    field,
    "number",
    0,
    false,
    validation,
    formStep
  );

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
  });

  $: labelClass =
    labelPos === "above" ? "" : `spectrum-FieldLabel--${labelPos}`;

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<div class="spectrum-Form-item" use:styleable={$component.styles}>
  {#if !formContext}
    <div class="placeholder">Form components need to be wrapped in a form</div>
  {:else}
    <label
      class:hidden={!label}
      for={fieldState?.fieldId}
      class={`spectrum-FieldLabel spectrum-FieldLabel--sizeM spectrum-Form-itemLabel ${labelClass}`}
    >
      {label || " "}
    </label>
    <div class="spectrum-Form-itemField">
      {#each { length: numberOfStars } as _, i}
        <Icon
          {color}
          {customSize}
          {size}
          {type}
          {variant}
          filled={hoverRating
            ? hoverRating >= i + 1
            : fieldState?.value >= i + 1}
          starId={i + 1}
          on:mouseover={() => (hoverRating = i + 1)}
          on:mouseleave={() => (hoverRating = null)}
          on:click={() => fieldApi?.setValue(i + 1)}
        />
      {/each}
      {#if !field}
        <div class="error">Please select a field</div>
      {/if}
      {#if fieldState?.error}
        <div class="error">{fieldState.error}</div>
      {/if}
    </div>
  {/if}
</div>

<style>
  .placeholder {
    color: var(--spectrum-global-color-gray-600);
  }
  label {
    white-space: nowrap;
  }
  label.hidden {
    padding: 0;
  }
  .spectrum-Form-itemField {
    position: relative;
    width: 100%;
  }
  .error {
    color: var(
      --spectrum-semantic-negative-color-default,
      var(--spectrum-global-color-red-500)
    );
    font-size: var(--spectrum-global-dimension-font-size-75);
    margin-top: var(--spectrum-global-dimension-size-75);
  }
  .spectrum-FieldLabel--right,
  .spectrum-FieldLabel--left {
    padding-right: var(--spectrum-global-dimension-size-200);
  }
</style>
