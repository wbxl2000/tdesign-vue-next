<template>
  <div>
    <t-space :size="24" direction="vertical">
      <div>
        <h3 :style="{ 'margin-bottom': '15px' }">加载中的图片</h3>
        <t-space>
          <t-space direction="vertical">
            <span>默认占位</span>
            <t-image
              v-model:key="loadingCount"
              src="https://tdesign.gtimg.com/demo/demo-image-1.png"
              :style="{ width: '284px', height: '160px' }"
            />
            <t-button variant="outline" :icon="renderIcon" @click="loadCount = loadCount + 1"> 重演 loading </t-button>
          </t-space>
          <t-space direction="vertical">
            <span>自定义占位</span>
            <t-image
              v-model:key="loadingCount"
              src="https://tdesign.gtimg.com/demo/demo-image-1.png"
              :style="{ width: '284px', height: '160px' }"
              :loading="renderCustomIcon"
            />
          </t-space>
        </t-space>
      </div>
      <div>
        <h3 :style="{ 'margin-bottom': '15px' }">加载失败的图片</h3>
        <t-space>
          <t-space direction="vertical">
            <span>默认错误</span>
            <t-image v-model:key="errorCount" src="" :style="{ width: '284px', height: '160px' }" />
            <t-button variant="outline" :icon="renderIcon" @click="errorCount = errorCount + 1"> 重演 error </t-button>
          </t-space>
          <t-space direction="vertical">
            <span>自定义错误</span>
            <t-image
              v-model:key="errorCount"
              src=""
              :style="{ width: '284px', height: '160px' }"
              :error="renderCustomIcon"
            />
          </t-space>
        </t-space>
      </div>
    </t-space>
  </div>
</template>

<script setup lang="jsx">
import { ref } from 'vue';
import { RefreshIcon, QrcodeIcon } from 'tdesign-icons-vue-next';

const loadingCount = ref(0);
const errorCount = ref(0);
const renderCustomIcon = <QrcodeIcon size="24" />;

const renderIcon = () => {
  return <RefreshIcon />;
};
</script>
