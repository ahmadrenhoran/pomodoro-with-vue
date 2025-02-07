<template>
    <div class="font-mono">
        <PomodoroContainer :bg="selectedTimer.bg">
            <TimerContainer :bg="selectedTimer.bgCard">
                <div class="flex justify-center gap-4 mb-4">
                    <TimerCategory v-for="timer in timerList" :key="timer.id" :timer="timer"
                        :selectTimer="selectTimer" />
                </div>
                <vue-countdown ref="countdown" :auto-start="false" class=" flex justify-center mb-4 text-8xl"
                    :time="selectedTimer.timer * 60 * 1000" v-slot="{ minutes, seconds }">
                    {{ minutes }}:{{ seconds }}
                </vue-countdown>

                <button v-if="!isCounting && !isPaused" @click="startCountdown"
                    :class="`bg-white ${selectedTimer.textColor}  border-b-4 border-slate-300 text-l font-semibold py-3 px-8 rounded-md shadow-md`">
                    START
                </button>
                <button v-else-if="isCounting && !isPaused" @click="pauseCountdown"
                    :class="`bg-white ${selectedTimer.textColor} border-b-4 border-slate-300 text-l  font-semibold py-3 px-8 rounded-md shadow-md`">
                    PAUSE
                </button>
                <button v-if="isPaused" @click="resumeCountdown"
                    :class="`bg-white ${selectedTimer.textColor} border-b-4 border-slate-300 text-l  font-semibold py-3 px-8 rounded-md shadow-md`">
                    RESUME
                </button>
            </TimerContainer>
        </PomodoroContainer>
    </div>
</template>

<script setup>
    import {
        ref, computed
    } from 'vue';
    import PomodoroContainer from '@/components/PomodoroContainer.vue';
    import TimerContainer from '@/components/TimerContainer.vue';
    import TimerCategory from '@/components/TimerCategory.vue';


    const timerList = ref([{
            id: 1,
            name: 'Pomodoro',
            timer: 25,
            isSelected: true,
            bg: 'bg-red-700',
            bgDark: 'bg-red-900',
            bgCard: 'bg-red-600',
            textColor: 'text-red-700',
        },
        {
            id: 2,
            name: 'Long Break',
            timer: 10,
            isSelected: false,
            bg: 'bg-teal-700',
            bgDark: 'bg-teal-900',
            bgCard: 'bg-teal-600',
            textColor: 'text-teal-700',
        },
        {
            id: 3,
            name: 'Break',
            timer: 5,
            isSelected: false,
            bg: 'bg-sky-700',
            bgDark: 'bg-sky-900',
            bgCard: 'bg-sky-600',
            textColor: 'text-sky-700',
        }
    ]);
    const countdown = ref(null);

    const selectedTimer = ref(timerList.value[0]);
    const isCounting = ref(false);
    const isPaused = ref(false);

    const startCountdown = () => {
        countdown.value.start();
        isCounting.value = true
        isPaused.value = false;
    };

    const pauseCountdown = () => {
        countdown.value.pause();
        isCounting.value = false;
        isPaused.value = true;
    };

    const resumeCountdown = () => {
        countdown.value.continue();
        isCounting.value = true;
        isPaused.value = false;
    };

    const selectTimer = (choiceId) => {

        countdown.value.abort();
        timerList.value.forEach(timer => timer.isSelected = false); // Reset semua isSelected
        isCounting.value = false;
        isPaused.value = false
        const newTimer = timerList.value.find(timer => timer.id === choiceId);
        if (newTimer) {
            newTimer.isSelected = true;
            selectedTimer.value = newTimer; // Simpan objek timer yang dipilih
        }
        console.log('Selected Timer:', selectedTimer.value);
    };
</script>
