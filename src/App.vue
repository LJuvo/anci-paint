<template>
  <section class="anci-container">
    <div class="anci-bg"></div>
    <div class="anci-container-content">
      <PaintMenu @select="selectMenu($event)" />

      <div class="anci-wrapper">
        <SeaPaint v-if="!isLoading" :paintOption="paintOption" />
      </div>
    </div>
  </section>
</template>
<script lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import Greet from "./components/Greet.vue";
import PaintMenu from "./components/PaintMenu.vue";
import SeaPaint from "./components/SeaPaint.vue";
import { defineComponent, reactive, ref } from "vue";

export default defineComponent({
  components: {
    // Greet,
    PaintMenu,
    SeaPaint,
  },
  setup() {
    let backgroundStyle = ref("");
    const theme = reactive({
      backgroundImage: "",
    });
    const paintOption = ref({});
    let isLoading = ref(true);
    return {
      theme,
      backgroundStyle,
      paintOption,
      isLoading: isLoading,
      selectMenu: (cell: any) => {
        isLoading.value = true;
        const bg = "url('/paint/" + cell.resourceId + "/thumb.jpg')";
        backgroundStyle.value = `background-image: "${bg}";`;
        theme.backgroundImage = bg;
        paintOption.value = cell;
        console.log(" paintOption ->", paintOption);
        isLoading.value = false;
      },
    };
  },
});
</script>

<style scoped>
.logo.vite:hover {
  filter: drop-shadow(0 0 2em #747bff);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #249b73);
}
.anci-container {
  width: 100%;
  height: 100%;

  position: relative;
  background: #000;
}
.anci-container-content {
  z-index: 1;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}
.anci-bg {
  background-repeat: no-repeat;
  background-size: cover;
  background-position: 50%;
  filter: blur(65px);
  opacity: 0.6;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 0;
  width: 100%;
  height: 100%;
  transform: translateZ(0);
  background-image: v-bind("theme.backgroundImage");
  background-color: #ffffff;
}
.anci-wrapper {
  flex: 1;
  height: 100%;
}
</style>
