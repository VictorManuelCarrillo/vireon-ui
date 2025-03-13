<template>
  <button ref="buttonRef" :class="buttonClasses" v-bind="$attrs">
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
  ripple?: boolean;
}

// ✅ Aquí definimos props + defaults
const props = withDefaults(defineProps<ButtonProps>(), {
  color: "primary",
  size: "md",
  shape: "normal",
  twClass: "",
  ripple: true,
  // nota: los boolean props los dejamos vacíos
});
// ripple

import { onMounted, ref } from 'vue'

const buttonRef = ref<HTMLButtonElement | null>(null)

onMounted(() => {
  if (!props.ripple || !buttonRef.value) return

  const el = buttonRef.value

  el.addEventListener('click', (e: MouseEvent) => {
    const ripple = document.createElement('span')
    ripple.classList.add('ripple')

    const rect = el.getBoundingClientRect()
    const size = Math.max(rect.width, rect.height)
    const x = e.clientX - rect.left - size / 2
    const y = e.clientY - rect.top - size / 2

    ripple.style.width = ripple.style.height = `${size}px`
    ripple.style.left = `${x}px`
    ripple.style.top = `${y}px`

    el.appendChild(ripple)

    setTimeout(() => {
      ripple.remove()
    }, 600)
  })
})




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
