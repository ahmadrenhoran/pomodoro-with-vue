<script setup>
defineProps({
    bg: {
        default: 'bg-teal-600'
    }
})
</script>

<template>
    <div :class="`${bg} p-6 rounded-lg shadow-lg`">
        <slot>

        </slot>
    </div>
</template>
