<template>
  <n-tabs class="all-box" size="large" justify-content="space-evenly" animated>
    <n-tab-pane class="no-padding" name="link" tab="我的链接">
      <ShortCut />
    </n-tab-pane>
  </n-tabs>
</template>

<script setup>
import { NTabs, NTabPane } from "naive-ui";
import ShortCut from "@/components/AllFunc/Box/ShortCut.vue";
</script>
