<script setup>
    import {
        defineProps
    } from 'vue';

    defineProps({
        bg: {
            type: String,
            default: '',
        }
    })
</script>

<template>
    <div :class="`${bg} flex justify-center items-center min-h-screen`">
        <div class="w-full max-w-md text-center text-white">
            <slot></slot>
        </div>
    </div>
</template>
