<template>
    <div class="font-mono">
        <PomodoroContainer :bg="selectedTimer.bg">
            <TimerContainer :bg="selectedTimer.bgCard">
                <div class="flex justify-center gap-4 mb-4">
                    <TimerCategory v-for="timer in timerList" :key="timer.id" :timer="timer"
                        :selectTimer="selectTimer" />
                </div>
                <div class="flex justify-center mb-4 text-8xl">
                    {{ minutes }}:{{ seconds }}
                </div>

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
import { ref, computed, onMounted } from 'vue';
import PomodoroContainer from '@/components/PomodoroContainer.vue';
import TimerContainer from '@/components/TimerContainer.vue';
import TimerCategory from '@/components/TimerCategory.vue';



const timerList = ref([
    { id: 1, name: 'Pomodoro', timer: 25, isSelected: true, bg: 'bg-red-700', bgDark: 'bg-red-900', bgCard: 'bg-red-600', textColor: 'text-red-700' },
    { id: 2, name: 'Long Break', timer: 10, isSelected: false, bg: 'bg-teal-700', bgDark: 'bg-teal-900', bgCard: 'bg-teal-600', textColor: 'text-teal-700' },
    { id: 3, name: 'Break', timer: 5, isSelected: false, bg: 'bg-sky-700', bgDark: 'bg-sky-900', bgCard: 'bg-sky-600', textColor: 'text-sky-700' }
]);

const selectedTimer = ref(timerList.value[0]);
let timer = null;
const minutes = ref(selectedTimer.value.timer.toString());
const seconds = ref("00");
const isCounting = ref(false);
const isPaused = ref(false);

onMounted(() => {
    const script = document.createElement("script");
    script.src = "https://cdn.jsdelivr.net/npm/easytimer.js@4.5.2/dist/easytimer.min.js";
    script.onload = () => {
        timer = new easytimer.Timer();
        timer.addEventListener('secondsUpdated', () => {
            const timeValues = timer.getTimeValues();
            minutes.value = String(timeValues.minutes).padStart(2, '0');
            seconds.value = String(timeValues.seconds).padStart(2, '0');
        });
    };
    document.body.appendChild(script);
});

const startCountdown = () => {
    timer.start({ countdown: true, startValues: { minutes: selectedTimer.value.timer } });
    isCounting.value = true;
    isPaused.value = false;
};

const pauseCountdown = () => {
    timer.pause();
    isCounting.value = false;
    isPaused.value = true;
};

const resumeCountdown = () => {
    timer.start();
    isCounting.value = true;
    isPaused.value = false;
};

const selectTimer = (choiceId) => {
    timer.stop();
    timerList.value.forEach(timer => timer.isSelected = false);
    isCounting.value = false;
    isPaused.value = false;
    
    const newTimer = timerList.value.find(timer => timer.id === choiceId);
    if (newTimer) {
        newTimer.isSelected = true;
        selectedTimer.value = newTimer;
    }

    minutes.value = String(selectedTimer.value.timer).padStart(2, '0');
    seconds.value = "00";
};
</script>