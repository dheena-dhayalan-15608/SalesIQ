<template>
  <div
    v-motion
    :initial="{ opacity: 0, scale: 0.5, x: 50 }"
    :enter="{ opacity: 1, scale: 1, x: 0, transition: { duration: 1000, ease: 'easeOut' } }"
    class="absolute z-50 pointer-events-none select-none
           top-[5px] right-2 w-16 h-16
           md:top-auto md:right-auto md:bottom-[120px] md:left-10 md:w-[400px] md:h-[500px]"
  >
    <!-- Mobile Rounded Avatar Wrapper -->
    <div class="md:hidden w-full h-full rounded-full border-2 border-cyber-blue shadow-[0_0_15px_#00f2ff] overflow-hidden bg-gradient-to-br from-[#1a1a2e] to-[#0f3460]">
       <img
        :src="avatarSrc"
        alt="Developer Avatar"
        class="w-full h-full object-cover"
        @error="handleImgError"
      />
    </div>

    <!-- Desktop Holographic Card Wrapper -->
    <div class="hidden md:flex relative w-[320px] h-[450px] bg-gradient-to-br from-[#1a1a2e] to-[#0f3460] rounded-sm shadow-[0_0_40px_rgba(0,242,255,0.2)] border border-white/10 overflow-hidden items-end justify-center">
      <!-- Glow effect -->
      <div class="absolute inset-0 bg-gradient-to-t from-cyber-blue/50 via-transparent to-transparent opacity-50" />
      
      <!-- The Hero Image -->
      <img
        :src="avatarSrc"
        alt="Developer Avatar"
        class="absolute bottom-0 w-full h-full object-cover opacity-100"
        @error="handleImgError"
      />
      
      <!-- Scan line overlay -->
      <div class="absolute top-5 left-5 right-5 pointer-events-none">
         <div class="w-full h-[3px] bg-cyber-blue opacity-50 mb-1"></div>
         <div class="font-mono text-[10px] text-cyber-blue uppercase tracking-widest">Biometric Scan Initialized</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

// Use a stable stylized avatar seed
const avatarSrc = ref('/src/dheena.jpg');

const handleImgError = (e: any) => {
  // Use a modern, high-quality 3D technical avatar illustration
  e.target.src = 'https://api.dicebear.com/7.x/notionists/svg?seed=Dheena&backgroundColor=0f3460';
};
</script>
