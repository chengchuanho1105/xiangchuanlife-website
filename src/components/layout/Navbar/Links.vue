<script setup lang="ts">
defineOptions({ name: 'NavbarLinks' });

import { computed, provide } from 'vue';
import { useBrandPath } from '@/composables/useBrandPath';

import type { NavbarItem } from '@/types/navbar';
import NavbarMenuItem from './NavbarMenuItem.vue'; // 導入 NavbarMenuItem

// Props 接收原始的導覽列項目數據
const props = defineProps<{
  items: NavbarItem[];
}>();

// 在這裡對頂層項目進行過濾和排序
const filteredAndSortedItems = computed(() => {
  return props.items
    .filter(item => item.displayInNavbar !== false) // 過濾掉不顯示的項目
    .sort((a, b) => (a.order || 0) - (b.order || 0)); // 排序
});

// 將 getBrandPath 提供給子元件使用
const { getBrandPath } = useBrandPath();
provide('getBrandPath', getBrandPath);
</script>

<template>
  <div class="hidden lg:flex">
    <ul class="flex items-center space-x-4">
      <NavbarMenuItem v-for="item in filteredAndSortedItems" :key="item.path" :item="item" />
    </ul>
  </div>
</template>

<style scoped></style>