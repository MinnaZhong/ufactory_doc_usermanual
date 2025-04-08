<script setup>
import { useData, useRoute } from 'vitepress';
import {  onMounted, ref, watch} from 'vue';
import {  FileDown } from "lucide-vue-next";


// pdf路由映射
const PDF_ROUTE_MAP = {
  "6_Axis_Force_Torque_Sensor":{
    "en-US": "https://www.ufactory.cc/wp-content/uploads/2023/11/UFACTORY-6-Axis-Force-Torque-Sensor-V2.2.0.pdf",
    "zh-CN": "https://www.cn.ufactory.cc/_files/ugd/896670_c45b72ffe8b44074ab44a500a63591fd.pdf",
  },
  "Bio_Gripper":{
    "en-US": "https://www.ufactory.cc/wp-content/uploads/2025/04/BIO-Gripper-G2-User-Manual-V2.6.0.pdf",
    "zh-CN": "https://www.cn.ufactory.cc/_files/ugd/896670_6649c39c6137474189235657766bd691.pdf",
  },

  "Linear_Motor":{
    "en-US": "https://www.ufactory.cc/wp-content/uploads/2023/05/UFACTORY-Linear-Motor-V2.0.04.pdf",
    "zh-CN": "https://www.cn.ufactory.cc/_files/ugd/896670_75b0c669a3f14963a38b100400716c63.pdf",
  },
  "xArm_Gripper":{
    "en-US": "https://www.ufactory.cc/wp-content/uploads/2025/04/xArm-Vacuum-Gripper-User-Manual-V2.5.2.pdf",
    "zh-CN": "https://www.cn.ufactory.cc/_files/ugd/896670_f9ede1a3f60e46eb9cb385d1e5748414.pdf",
  },
  "xArm_Vacuum_Gripper":{
    "en-US": "https://www.ufactory.cc/wp-content/uploads/2023/04/xArm-Vacuum-Gripper-User-Manual-V1.6.1.pdf",
    "zh-CN": "https://www.cn.ufactory.cc/_files/ugd/896670_d1bb14ad508243e1bac6b812cbdffd90.pdf",
  },
}


// 控制按钮显示
const showButton = ref(false);
const route = useRoute()
const data = useData()

const isEnglish = ref(false)

watch(() => data.lang, (data) => {
  isEnglish.value = (data.value === 'en-US')
}, {
  deep: true,
  immediate: true
})


onMounted(() => {
  // 只在首页和文档页面显示按钮
  if (window.location.pathname !== '/' && !window.location.pathname.startsWith('/')) {
    showButton.value = false;
  } else {
    showButton.value = true;
  }
});

function onOpenPdf() {
  Object.keys(PDF_ROUTE_MAP).forEach((key) => {
    if (route.path.includes("/" + key + "/")) {
      const url = PDF_ROUTE_MAP[key][data.lang.value];
      if (url) {
        window.open(url);
      }
    }
  });
}

</script>
<template>

  <div class="export-btn">
    <!-- 根据 showButton 的值来控制按钮是否显示 -->
    <button v-if="showButton" @click="onOpenPdf" class="export-pdf-button" >
      <FileDown />
      {{isEnglish ? "Download PDF" :"PDF下载"}}
    </button>
  </div>
</template>

<style scoped>
.export-modal{
  text-align: center;
  font-size: 18px;
  margin: 20px;
}

.export-button{
  width: 100%;
  display: flex;
  row-gap: 10px;
  justify-content: center;
}
</style>
