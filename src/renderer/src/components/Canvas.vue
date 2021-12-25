<template lang="">
  <div class="background">
      <div :style="canvasRect">
            <canvas id="canvas"></canvas>
        </div>
        <div class="selected-tag" v-show="showTag">
            <n-tag>
                Selected Image: {{selectedName}}
            </n-tag>
        </div>
      <n-space align="center" justify="center">
          <n-button color="#66CCCC" @click="addImageClick">
              Add Image
          </n-button>

          <n-button color="#ff69b4" @click="deleteImageClick">
              Delete Image
          </n-button>
      </n-space>
        
      <input id="file" style="display: none" type="file" @change="addImage()">
  </div>
</template>

<script lang="ts">
import { onMounted, reactive, ref } from "vue";
import { fabric } from "Fabric";
import { NButton, NSpace, useMessage, NTag} from "naive-ui";
import { Canvas } from "fabric/fabric-impl";
// import { Canvas } from 'fabric/fabric-impl';
export default {
  components: {
    NButton,
    NSpace,
    NTag
  },
  setup() {
    const message = useMessage()
    let card: Canvas
    let selectedName = ref('')
    let showTag = ref(false)
    const canvasRect = reactive({
      height: "0px",
      width: "0px",
      marginTop: "0px",
      marginLeft: "0px",
    })
    let canvasW: number
    let canvasH: number
    onresize = async () => {
      updateCanvasSize();
    };
    onMounted(() => {
      initCanvas();
    });
    const initCanvas = () => {
      card = new fabric.Canvas("canvas");
      card.on('selection:cleared', () => {
          showTag.value = false
      })
      updateCanvasSize();
    };
    const updateCanvasSize = () => {
      canvasW = window.innerWidth / 2;
      canvasH = window.innerHeight / 2;
      canvasRect.height = canvasH + "px";
      canvasRect.width = canvasW + "px";
      canvasRect.marginTop = canvasH / 2 + "px";
      canvasRect.marginLeft = canvasW / 2 + "px";
      card.setWidth(canvasW);
      card.setHeight(canvasH);
    };
    const addImageClick = () => {
      let file = document.getElementById("file");
      file?.click();
    };
    function addImage() {
      let files = document.getElementById("file") as HTMLInputElement;
      let file = files!.files![0];
      let reg = /(png|jpg|gif|jpeg)$/;
      if (!reg.test(file.type)) {
        message.error("请选择（.png|.jpg|.gif|.jpeg）文件！");
        return;
      }
      let r = new FileReader();
      r.readAsDataURL(file);
      r.onload = function () {
        let image = new Image();
        image.src = r.result as string;
        image.onload = () => {
          let img = new fabric.Image(image);
          img.on("selected", (e) => {
            console.log(e);
            e.target;
            selectedName.value = e.target?.name || ''
            showTag.value = true;
          });
          img.name = file.name;
          // img.hasControls = false;
          img.borderColor = "#66cccc";
          imageScale(img);
          card.add(img);
          card.setActiveObject(img);
          card.renderAll();
        };
      };
    }
    const imageScale = (image: fabric.Image) => {
      let imageW = image.width || 0;
      if (imageW > canvasW * 0.5) {
        image.scaleToWidth(canvasW * 0.5);
      }
    };
    const deleteImageClick = () => {
      let obj = card.getActiveObject();
      if (!obj) {
          message.error("请选择图片后删除！");
          return
      }
      card.remove(obj);
      showTag.value = false
      message.info("delete" + obj.name);
    };

    return {
      canvasRect,
      addImageClick,
      addImage,
      deleteImageClick,
      selectedName,
      showTag
    };
  },
};
</script>

<style scoped>
.background {
  height: 100%;
  margin: 0;
  padding: 0;
}

#canvas {
  box-shadow: 0px 0px 10px 2px #66cccc;
}
.selected-tag {
  position: fixed;
  left: 78%;
  top: 25%;
  width: 8em;
}
</style>
