<template>
  <div
    v-motion
    :initial="{ opacity: 0, y: 50 }"
    :enter="{ opacity: 1, y: 0 }"
    class="absolute bottom-2 md:bottom-6 left-2 md:left-1/2 right-2 md:right-auto md:-translate-x-1/2 w-auto md:w-[800px] min-h-[80px] md:h-[100px] bg-glass-bg backdrop-blur-[20px] border border-glass-border p-4 md:p-6 flex flex-col justify-center z-50 pointer-events-auto shadow-[0_10px_30px_rgba(0,0,0,0.5)]"
  >
    <div class="font-mono text-cyber-blue text-[11px] md:text-[13px] leading-relaxed">
      {{ displayedText }}
      <span v-if="isTyping" class="inline-block w-2 h-[12px] md:h-[15px] bg-cyber-blue ml-1 align-middle"></span>
    </div>
    <div class="absolute bottom-1 md:bottom-2 right-2 md:right-4 font-mono text-[7px] md:text-[9px] opacity-40 uppercase tracking-widest">
      [ Authorization Confirmed ]
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';

const props = defineProps<{
  text: string;
}>();

const emit = defineEmits(['complete']);

const displayedText = ref('');
const isTyping = ref(false);
let timeoutId: any = null;

const type = (fullText: string, i: number) => {
  if (i < fullText.length) {
    displayedText.value = fullText.substring(0, i + 1);
    timeoutId = setTimeout(() => type(fullText, i + 1), 25);
  } else {
    isTyping.value = false;
    emit('complete');
  }
};

const startTyping = () => {
  if (timeoutId) clearTimeout(timeoutId);
  displayedText.value = '';
  isTyping.value = true;
  type(props.text, 0);
};

watch(() => props.text, startTyping);

onMounted(startTyping);
</script>
