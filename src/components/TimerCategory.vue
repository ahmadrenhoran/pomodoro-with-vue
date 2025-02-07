<template>
    <button 
        class="px-3 py-1 border-b-2 transition duration-200 rounded-sm"
        :class="[
            timer.isSelected 
                ? `${timer.bgDark} border-white text-white` 
                : 'border-transparent text-white hover:border-white'
        ]"
        @click="selectTimer(timer.id)"
    >
        {{ timer.name }}
    </button>
</template>

<script setup>
defineProps({
    timer: Object,
    selectTimer: Function,
});
</script>