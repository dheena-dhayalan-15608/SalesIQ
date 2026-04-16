<template>
  <div
    v-motion
    :initial="{ opacity: 0, x: 50 }"
    :enter="{ opacity: 1, x: 0 }"
    class="flex flex-col gap-2 md:gap-4 p-4 md:p-0 w-full md:w-[350px]"
  >
    <h2 class="font-mono text-[10px] md:text-xs mb-1 md:mb-4 text-cyber-pink tracking-widest uppercase text-right md:text-left">
      Mission Archives
    </h2>
    
    <button
      v-for="(mission, index) in missions"
      :key="mission.id"
      v-motion
      :initial="{ opacity: 0, x: 20 }"
      :enter="{ opacity: 1, x: 0, transition: { delay: 200 + index * 100 } }"
      @click="$emit('select', mission)"
      class="group relative text-left w-full"
    >
      <div class="relative bg-white/[0.03] backdrop-blur-[10px] border-l-4 border-cyber-blue border-r border-white/[0.05] p-3 md:p-5 cursor-pointer transition-all hover:bg-cyber-blue/15 transform md:-skew-x-[10deg]">
        <div class="transform md:skew-x-[10deg]">
          <div class="font-sans font-bold text-xs md:text-sm uppercase tracking-wider mb-0 md:mb-1">
            {{ mission.title }}
          </div>
          <div class="font-mono text-[8px] md:text-[10px] text-white/60">
            {{ mission.id === 'salesiq' ? 'RT-INTEGRATION // OK' : (mission.id === 'console' ? 'MEM-OPTIMIZATION // OK' : 'DATA ACCESSED // OK') }}
          </div>
        </div>
      </div>
    </button>

    <button
      v-motion
      :initial="{ opacity: 0, x: 20 }"
      :enter="{ opacity: 1, x: 0, transition: { delay: missions.length * 100 + 300 } }"
      @click="reload"
      class="mt-2 md:mt-4 self-start font-mono text-[8px] md:text-[10px] bg-white/[0.03] backdrop-blur-[10px] border-l-4 border-gray-600 border-r border-white/[0.05] p-3 md:p-5 w-full text-left opacity-50 hover:opacity-100 transition-opacity transform md:-skew-x-[10deg]"
    >
      <div class="transform md:skew-x-[10deg]">
        REBOOT SYSTEM
      </div>
    </button>
  </div>
</template>

<script setup lang="ts">
interface Mission {
  id: string;
  title: string;
  description: string;
}

defineProps<{
  missions: Mission[];
}>();

defineEmits(['select']);

const reload = () => window.location.reload();
</script>
