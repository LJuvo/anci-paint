<template>
  <aside class="paint-menu">
    <ul>
      <li
        v-for="(item, key) in menuList"
        :key="key"
        :class="{ 'paint-menu-acive': currentId === item.id }"
        @click="selectMenuCell(item.id)"
      >
        <img
          class="paint-menu-pic"
          :src="'/paint/' + item.resourceId + '/thumb.jpg'"
        />
        <div class="paint-menu-title">{{ item.name }}</div>
      </li>
    </ul>
  </aside>
</template>
<script lang="ts">
import { defineComponent, reactive, ref, toRefs } from "vue";

export default defineComponent({
  setup(_, { emit }) {
    let menuList = [
      {
        id: "60890cfc611ecdd2f4e3eecb",
        resourceId: "5be3970c8ed7f411e26a5646",
        age: "晋",
        ageDetail: "东晋",
        author: "顾恺之",
        levelScore: 249.07,
        maxlevel: 18,
        minlevel: 10,
        name: "女史箴图全卷（唐摹本）",
        size: { width: 79575, height: 3130 },
        thumbTileSize: { width: 512, height: 391 },
      },
      {
        id: "62badd97ca9e14aecd7e768d",
        resourceId: "62bac3408911e8a824b81853",
        age: "清",
        ageDetail: "",
        author: "华嵒",
        levelScore: 123.7,
        maxlevel: 18,
        minlevel: 13,
        name: "午日锺馗图轴.清华嵒绘.纸本设色.7840x15778像素.台北故宫博物院藏",
        size: { width: 7840, height: 15778 },
        thumbTileSize: { width: 245, height: 493 },
      },
    ];
    let currentId = ref("");

    const data = reactive({
      selectMenuCell: (id: string) => {
        currentId.value = id;
        const cell = menuList.find((o) => o.id === id);
        emit("select", cell);
      },
    });
    const refsData = toRefs(data);
    return { menuList: menuList, currentId: currentId, ...refsData };
  },
  mounted() {
    this.$nextTick(() => {
      this.selectMenuCell(this.menuList[0].id);
    });
  },
});
</script>
<style scoped>
.paint-menu {
  width: 240px;
  height: calc(100% - 48px);
  overflow: hidden;
  overflow-y: auto;
  color: #ffffff;
  z-index: 2;
  padding: 24px;
  background: linear-gradient(90deg, rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.05));
}
.paint-menu-acive {
  color: rebeccapurple;
}
.paint-menu ul {
  margin: 0;
  padding: 0;
}
.paint-menu ul li {
  cursor: pointer;
  width: 100%;
  height: 120px;
  overflow: hidden;
  margin-bottom: 24px;
  position: relative;
}
.paint-menu ul li:last-child {
  margin-bottom: 0;
}
.paint-menu-pic {
  width: 100%;
}
.paint-menu-title {
  /* height: 40px; */
  width: calc(100% - 32px);
  position: absolute;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.3);
  color: #ffffff;
  padding: 8px 16px;
  font-size: 14px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>
