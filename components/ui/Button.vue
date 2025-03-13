<template>
  <button :class="buttonClasses" v-bind="$attrs">
    <slot />
  </button>
</template>

<script setup lang="ts">
interface ButtonVariants {
  filled?: boolean;
  plastic?: boolean;
  outlined?: boolean;
  text?: boolean;
  tonal?: boolean;
}
interface ButtonProps extends ButtonVariants {
  color?: "primary" | "secondary" | "success" | "warning" | "error" | "info";
  size?: "xs" | "sm" | "md" | "lg" | "xl";
  shape?: "normal" | "rounded" | "pill" | "xl";
  twClass?: string;
  gradient?: boolean | string;
}

// ✅ Aquí definimos props + defaults
const props = withDefaults(defineProps<ButtonProps>(), {
  color: "primary",
  size: "md",
  shape: "normal",
  twClass: "",
  // nota: los boolean props los dejamos vacíos
});

const resolvedVariant = computed(() => {
  const variants = [
    "outlined",
    "plastic",
    "text",
    "tonal",
    "filled",
  ] as const;
  return variants.find((v) => props[v]) || "filled";
});

const buttonClasses = computed(() => [
  "btn",
  `btn--variant-${resolvedVariant.value}`,
  props.color && `btn--color-${props.color}`,
  props.size && `btn--size-${props.size}`,
  props.shape && `btn--shape-${props.shape}`,
  props.twClass,
  props.gradient && `btn--gradient`
  ]);
</script>
