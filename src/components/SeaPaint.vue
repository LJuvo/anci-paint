<template>
  <div class="sea-paint">
    <div id="openseadragonWrapper" style="width: 100%; height: 100%"></div>
    <!-- <div class="toolbar-pane">
      <div id="toolbarDiv">
        <div>
          <div id="zoom-in">放大</div>
          <div id="zoom-out">缩小</div>
          <div id="home">默认</div>
          <div id="full-page">全屏</div>
        </div>
      </div>
    </div> -->
  </div>
</template>
<script lang="ts">
import { defineComponent, watch } from "vue";
import OpenSeadragon from "openseadragon";

export default defineComponent({
  props: {
    paintOption: {
      type: Object,
      default: () => {},
    },
  },
  setup(props) {
    var initOpenSeadragon = (options: any) => {
      let viewer = OpenSeadragon(options);
      return viewer;
    };

    var initViewer = () => {
      const ele = document.getElementById("openseadragonWrapper");
      ele!.innerHTML = "";
      const baseOption = props.paintOption;
      console.log("baseOption ->", baseOption);

      if (baseOption && baseOption.id) {
        initOpenSeadragon({
          id: "openseadragonWrapper",
          //   toolbar: "toolbarDiv",
          prefixUrl: "/openseadragon/images/",
          navigatorSizeRatio: 0.25,
          wrapHorizontal: false,
          tileSources: {
            height: baseOption.size.height,
            width: baseOption.size.width,
            tileSize: 512,
            minLevel: baseOption.minlevel,
            maxLevel: baseOption.maxlevel,
            Size: {
              //图片总大小
              Height: baseOption.size.height,
              Width: baseOption.size.width,
            },
            getTileUrl: function (level: number, x: number, y: number) {
              //   console.log("level,x,y->", level, x, y);
              let mX = x;
              let mY = y;
              // if (baseOption.size.height > baseOption.size.width) {
              //   mX = y;
              //   mY = x;
              // }
              return (
                "/paint/" +
                baseOption.resourceId +
                "/" +
                level +
                "/" +
                mX +
                "_" +
                mY +
                ".jpg"
              );
            },
          },
          constrainDuringPan: true,
          showNavigator: true, // 展示导航图
          //   zoomInButton: "zoom-in", //放大
          //   zoomOutButton: "zoom-out", //缩小
          //   homeButton: "home", //恢复默认
          //   fullPageButton: "full-page", //全屏
          autoHideControls: true,
        });
      }
    };

    watch(
      () => props.paintOption,
      (baseOption) => {
        console.log("watch baseOption ->", baseOption);
        initViewer();
      }
    );

    return {
      initOpenSeadragon,
      initViewer,
    };
  },
  mounted() {
    this.initViewer();
  },
});
</script>

<style scoped>
.sea-paint {
  position: relative;
  width: 100%;
  height: 100%;
}
.contentDiv {
  width: 100%;
  height: 100%;
}
.toolbar-pane {
  position: absolute !important;
  z-index: 999;
  top: 24px;
  left: 24px;
}
#toolbarDiv {
}
#zoom-in {
  margin-right: 20px;
}

#zoom-out {
  margin-right: 20px;
}

#home {
  margin-right: 20px;
}

#full-page {
}
</style>
