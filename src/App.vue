<template>
  <div class="relative w-screen h-screen overflow-hidden text-white font-sans">
    <!-- 3D Background -->
    <ThreeBackground />

    <!-- Decorative Particles Simulation -->
    <div class="absolute inset-0 pointer-events-none opacity-10 bg-[radial-gradient(white_1px,transparent_1px),radial-gradient(white_1px,transparent_1px)] bg-[length:50px_50px,100px_100px] bg-[position:0_0,25px_25px]" />

    <!-- UI Overlay -->
    <div class="relative z-10 w-full h-full flex flex-col p-4 md:p-10 pointer-events-none">
      
      <!-- Header -->
      <header class="flex flex-col md:flex-row justify-between items-start md:items-center z-10 gap-4 md:gap-0">
        <div class="flex flex-col">
          <div class="font-[900] tracking-[2px] md:tracking-[5px] text-base md:text-lg font-orbitron">ZOHO.OPERATOR</div>
          <div class="font-press-start text-[6px] md:text-[8px] text-cyber-blue mt-1 uppercase">OPERATOR: DHEENA</div>
        </div>
        <div class="flex gap-3 md:gap-5 font-mono text-[8px] md:text-[10px] uppercase tracking-[1px] md:tracking-[2px] text-cyber-blue">
          <span>CPU: 42%</span>
          <span>SYNC: ESTABLISHED</span>
          <span class="hidden sm:inline">NODE: 0x88F2</span>
        </div>
      </header>

      <transition name="fade" mode="out-in">
        <div v-if="!hasStarted" key="start-screen" class="flex-1 flex items-center justify-center p-4">
          <div
            v-motion
            :exit="{ opacity: 0, scale: 2, filter: 'blur(20px)' }"
            class="flex flex-col items-center gap-6 md:gap-8 pointer-events-auto"
          >
            <div class="relative">
              <h1 
                v-motion
                :initial="{ opacity: 0, scale: 0.5 }"
                :enter="{ opacity: 1, scale: 1 }"
                class="font-press-start text-lg md:text-2xl text-center leading-relaxed text-cyber-blue drop-shadow-[0_0_10px_#00f2ff]"
              >
                DHEENA'S JOURNEY
              </h1>
              <div class="absolute -bottom-4 left-0 w-full h-px bg-cyber-blue blur-sm animate-pulse" />
            </div>
            
            <button
              @click="startAdventure"
              class="px-8 md:px-12 py-4 md:py-6 border-2 border-white font-press-start text-[10px] md:text-sm hover:bg-white hover:text-black transition-all cursor-pointer group relative overflow-hidden"
            >
              INITIALIZE CORE
              <div class="absolute inset-0 bg-white/20 -z-10 blur-xl opacity-0 group-hover:opacity-100 transition-opacity" />
            </button>
          </div>
        </div>

        <div v-else key="game-screen" class="w-full h-full relative mt-4 md:mt-0">
          <!-- Stats Panel -->
          <div
            v-motion
            :initial="{ opacity: 0, x: -20 }"
            :enter="{ opacity: 1, x: 0, transition: { delay: 500 } }"
            class="absolute md:left-[400px] md:top-[120px] left-0 top-[60px] md:top-0 w-full md:w-56 bg-white/5 backdrop-blur-md border border-white/10 p-3 md:p-4 space-y-4 z-30"
          >
            <div class="space-y-1">
              <div class="text-[8px] font-mono text-white/40 uppercase tracking-widest">Skill Multiplier</div>
              <div class="h-1 bg-white/10 rounded-full overflow-hidden">
                <div 
                  class="h-full bg-cyber-blue transition-all duration-1000"
                  :style="{ width: '85%' }"
                ></div>
              </div>
            </div>
            <div class="grid grid-cols-2 gap-4">
              <div>
                <div class="text-[8px] font-mono text-white/40 uppercase">Agility</div>
                <div class="font-mono text-xs text-cyber-blue">92%</div>
              </div>
              <div>
                <div class="text-[8px] font-mono text-white/40 uppercase">Intel</div>
                <div class="font-mono text-xs text-cyber-blue">98%</div>
              </div>
            </div>
            <div class="pt-4 border-t border-white/10">
              <div class="text-[8px] font-mono text-cyber-pink uppercase font-bold">Tech Stack</div>
              <div class="text-[9px] font-mono mt-2 gap-2 flex flex-wrap opacity-80 uppercase leading-relaxed">
                VUE • JAVA • NODE.JS • SQL • DELUGE
              </div>
            </div>
            <div class="pt-2 hidden md:block">
               <div class="text-[8px] font-mono text-white/40 uppercase">Assigned Hub</div>
               <div class="font-mono text-[9px] text-cyber-blue">ZOHO_CORP_HQ</div>
            </div>
          </div>

          <!-- Hero Avatar -->
          <HeroAvatar />

          <!-- Mission Selection -->
          <div class="absolute right-0 top-[220px] md:top-1/2 md:-translate-y-1/2 pointer-events-auto z-40 w-full md:w-auto">
            <MissionMenu 
              :missions="MISSIONS" 
              @select="handleSelectMission"
            />
          </div>

          <!-- Dialogue Box -->
          <DialogueBox :text="currentDialogue" />
        </div>
      </transition>

      <!-- Ambient Scanline Effect -->
      <div class="fixed inset-0 pointer-events-none bg-[linear-gradient(rgba(18,16,16,0)_50%,rgba(0,0,0,0.25)_50%),linear-gradient(90deg,rgba(255,0,0,0.06),rgba(0,255,0,0.02),rgba(0,0,255,0.06))] bg-[length:100%_2px,3px_100%] z-50 opacity-20" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import ThreeBackground from './components/ThreeBackground.vue';
import DialogueBox from './components/DialogueBox.vue';
import MissionMenu from './components/MissionMenu.vue';
import HeroAvatar from './components/HeroAvatar.vue';

const MISSIONS = [
  {
    id: 'profile',
    title: 'Profile: DHEENA',
    description: 'BIOMETRIC DATA: DHEENA. ROLE: Member Technical Staff. SPECIALIZATION: System Architect. TECH STACK: Vue.js, Java, SQL, Deluge. STATUS: ACTIVE.'
  },
  {
    id: 'salesiq',
    title: 'Zoho SalesIQ Logic',
    description: 'MISSION DATA: Built seamless integration bridges within Zoho SalesIQ. Connected user engagement logs with CRM databases. STATUS: SUCCESS.'
  },
  {
    id: 'console',
    title: 'Console Core Logic',
    description: 'MISSION DATA: Engineered a high-efficiency console application during the Zoho Schools intensive program. MEMORY OPTIMIZATION: 99%.'
  },
  {
    id: 'education',
    title: 'Zoho Schools Training',
    description: 'MISSION DATA: Completed intensive training at Zoho Schools. Specialized in Scalable Architecture and backend efficiency protocols.'
  }
];

const currentDialogue = ref('SYSTEM INITIALIZED. WAITING FOR COMMAND...');
const hasStarted = ref(false);

const startAdventure = () => {
  hasStarted.value = true;
  currentDialogue.value = 'Greetings, authorized user DHEENA. System migration to Vue engine successful. I am now serving as your Member Technical Staff interface.';
};

const handleSelectMission = (mission: any) => {
  currentDialogue.value = mission.description;
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
