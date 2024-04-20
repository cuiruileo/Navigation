<template>
  <!-- 一言 -->
  <div :class="[
      'hitokoto',
      status.siteStatus !== 'normal' ? 'hidden': null,
    ]"
  >
    <div class="text">{{ hitokotoData?.text }}</div>
    <div class="from">「 {{ hitokotoData?.from }} 」</div>
  </div>
</template>

<script setup>
import { getCurrentTime } from "@/utils/timeTools";
import { ref, onMounted, watch } from "vue";
import { statusStore, setStore } from "@/stores";
import { getHitokoto } from "@/api";

const set = setStore();
const status = statusStore();

const timeData = getCurrentTime(true, false);

const hitokotoData = ref(null);
const hitokotoType = set.hitokotoType;

// 获取一言数据
const getHitokotoData = async () => {
  try {
    const getData = await getHitokoto(hitokotoType);
    hitokotoData.value = {
      text: getData.hitokoto,
      from: getData.from,
    };
  } catch (error) {
    hitokotoData.value = {
      text: "现在是 " + timeData.month + " 月 " + timeData.day + " 日" + timeData.weekday + "，" + timeData.hour + " 时 " + timeData.minute + " 分···星空，晴天，微风···听得见吗？我喜欢你。请回答我呀",
      from: "あだち 充",
    };
  }
};

// 监听配置发生改变
watch(
  () => [set.hitokotoType],
  () => {
    getHitokotoData();
  },
);

onMounted(() => {
  getHitokotoData();
});
</script>

<style lang="scss" scoped>
.hitokoto {
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 620px;
  width: calc(100% - 90px);
  transform: translateY(120px);
  color: var(--main-text-color);
  transition:
    transform 0.3s,
    opacity 0.5s,
    margin-bottom 0.3s;
  z-index: 1;

  &.hidden {
    transform: translateY(-55px);
    // transform: translateY(-24vh);
    opacity: 0;
  }

  .text{
    display: flex;
    margin-bottom: 10px;
    justify-content: center;
    text-align: center;
    font-size: 1.15rem;
    opacity: 0.7;
    text-shadow: var(--main-text-shadow);
  }
  .from{
    display: flex;
    justify-content: center;
    font-size: 1rem;
    opacity: 0.7;
    text-shadow: var(--main-text-shadow);
  }
}
</style>
