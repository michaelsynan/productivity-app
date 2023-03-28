<template>
    <div class="flex flex-col">
      <div class="h-screen flex items-center justify-center z-30" id="details" :class="{ fixed: fixed }">
     
        <div id="maindetials" class="grid grid-cols-3 gap-1 items-center h-full">
          <div ref="detailInfo" :class="currentbackground" class="col-span-1 text-white h-full px-20 py-40">
            <div class="text-lg text-stone-50 font-black uppercase pb-2">Test</div>
            <div class="text-base text-stone-200">Sandwich</div>
          </div>
          <div class="col-span-2 h-full w-full overflow-hidden">
            <div class=" opacity-0 h-full" ref="detailImage">
              <img class=" object-cover hover:brightness-110 items-center shadow h-full bg-black"/>
            </div>
          </div>
        </div>
      </div>
      <div v-if="fixed" :style="{ height: `${initialHeight + 500}px` }"></div>
  
      <div v-if="currentSection" class="h-screen flex items-center justify-center bg-slate-900 z-40" id="details">
        <div class="text-9xl">HELLO</div>
      </div>
      <div v-if="currentSection" class="h-screen flex items-center justify-center bg-slate-900 z-40" id="details">
        <div class="text-9xl">WORLD</div>
      </div>
    </div>
  
  </template>
  
  
  <script lang="ts" setup>
  import { ref, computed, watch, onUnmounted, watchEffect, nextTick, onMounted, onBeforeUnmount } from "vue";
  import { useSectionsStore } from "@/store/sections";
  

  
  onMounted(async () => {
    await nextTick();
    if (detailsRef.value) {
      initialTop.value = detailsRef.value.getBoundingClientRect().top;
      initialHeight.value = detailsRef.value.offsetHeight;
    }
    window.addEventListener("scroll", handleScroll);
  });
  
  onBeforeUnmount(() => {
    window.removeEventListener("scroll", handleScroll);
  });
  
  const fixed = ref(false);
  const initialTop = ref(0);
  const initialHeight = ref(0);
  
  function handleScroll() {
    if (window.scrollY >= initialTop.value) {
      fixed.value = true;
    } else {
      fixed.value = false;
    }
  }
  
  
  onMounted(async () => {
    await nextTick();
 
  });
  

  const detailInfo = ref<HTMLElement | null>(null);
  const detailImage = ref<HTMLElement | null>(null);
  
  const options = {
    root: null,
    threshold: 0.25,
    once: true,
  };
  
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add("fadeIn");
        setTimeout(() => {
          entry.target.classList.remove("delay-500");
        }, 1000);
      } else {
        entry.target.classList.remove("fadeIn");
        entry.target.classList.add("delay-500");
      }
    });
  }, options);
  
  watch(
    [detailInfo, detailImage],
    ([infoEl, imageEl]) => {
      if (infoEl) {
        observer.observe(infoEl);
      }
      if (imageEl) {
        observer.observe(imageEl);
      }
    },
    { immediate: true }
  );
  
  onUnmounted(() => {
    observer.disconnect();
  });
  </script>
  
  <style scoped>

  .delay-1000 {
    transition-delay: 0.3s;
  }
  
  .fixed {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 10;
  }
  
  .fadeIn {
    opacity: 1;
    transform: translate(0, 0);
    transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
  }
  
  .delay-1000 {
    transition-delay: 0.3s;
  }
  </style>
  