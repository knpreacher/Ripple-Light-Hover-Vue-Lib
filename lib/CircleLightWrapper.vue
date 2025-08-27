<script setup lang="ts">
import { onMounted, onUnmounted, ref, computed, defineComponent, h } from 'vue';


const props = withDefaults(defineProps<{
  color?: string,
  disabled?: boolean,
  zIndex?: number,
  wrapperExtraClass?: string,
  size?: number,
  borderCircleSize?: number
  innerCircleSize?: number
  innerCircleOpacity?: number
}>(), {
  color: 'rgb(0,111,255)',
  borderCircleSize: 300,
  innerCircleOpacity: 0.05,
  innerCircleSize: 300,
  zIndex: 0,
  size: 2
})

const INITIAL_POSITION_VALUE = -9999

let mousePositionX: number = INITIAL_POSITION_VALUE
let mousePositionY: number = INITIAL_POSITION_VALUE

const pixelSize = computed(() => `${props.size}px`)
const pixelBorderCircleSize = computed(() => `${props.borderCircleSize}px`)
const pixelInnerCircleSize = computed(() => `${props.innerCircleSize}px`)

const rippleContainer = ref<HTMLElement | null>(null)
const mouseMove = async (e: MouseEvent) => {
    if (!rippleContainer.value) return
    mousePositionX = e.clientX
    mousePositionY = e.clientY
}

function updateRippleItems() {
    if (props.disabled) {
        mousePositionX = INITIAL_POSITION_VALUE
        mousePositionY = INITIAL_POSITION_VALUE
        return
    }
    if (!rippleContainer.value) return
    const children = rippleContainer.value.querySelectorAll('.ripple-light-item__wrapper')
    for (let i = 0; i < children.length; i++) {
        const el = children[i] as HTMLElement
        const rect = el.getBoundingClientRect()
        el.style.setProperty('--x', `${mousePositionX - rect.left}px`)
        el.style.setProperty('--y', `${mousePositionY - rect.top}px`)
        // el.style.setProperty('--x', `${mousePositionX - el.offsetLeft + window.scrollX}px`)
        // el.style.setProperty('--y', `${mousePositionY - el.offsetTop + window.scrollY}px`)
    }
    requestAnimationFrame(updateRippleItems)
}

function onMouseLeave() {
    mousePositionX = INITIAL_POSITION_VALUE
    mousePositionY = INITIAL_POSITION_VALUE
}

onMounted(() => {
    document.addEventListener('mousemove', mouseMove)
    document.addEventListener('mouseleave', onMouseLeave)
    updateRippleItems()
})

onUnmounted(() => {
    document.removeEventListener('mousemove', mouseMove)
    document.removeEventListener('mouseleave', onMouseLeave)
})

const wrapperComponent = defineComponent((_props, ctx) => {
    return () => h('div', { class: `ripple-light-item__wrapper ${props.wrapperExtraClass ?? ''}` }, [
        ctx.slots.default?.()
    ])
})

defineSlots<{
    default: (props: { cls: string, wrapper: any }) => any
}>()
</script>

<template>
    <div class="ripple-light-container" ref="rippleContainer">
        <slot v-bind="{ cls: 'ripple-light-item', wrapper: wrapperComponent }"></slot>
    </div>
</template>

<style>
.ripple-light-container {
    --x: -99999px;
    --y: -99999px;
}

.ripple-light-item {
    position: relative;
    z-index: 1;
}

.ripple-light-item__wrapper {
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: v-bind(pixelSize);
    z-index: v-bind(zIndex);
    background: radial-gradient(v-bind(pixelBorderCircleSize) circle at var(--x) var(--y), v-bind(color) 0, transparent 100%);
    overflow: hidden;
}

.ripple-light-item {
    z-index: v-bind(zIndex);
}

.ripple-light-item::before {
    opacity: v-bind(innerCircleOpacity);
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: inherit;
    z-index: -1;
    background: radial-gradient(v-bind(pixelInnerCircleSize) circle at var(--x) var(--y), v-bind(color) 0, transparent 100%);
    overflow: hidden;
}
</style>
