<script setup lang="ts">
import { Drawer, DrawerContent } from "@progress/kendo-vue-layout";
import { useLocalStorage } from "@vueuse/core";
import { computed, ref } from "vue";
import { useRouter } from 'vue-router';

const router = useRouter();

const expanded = useLocalStorage("vue-forge-drawer-expanded", true);
const expandedIcon = computed(() =>
  expanded.value ? "k-i-arrow-chevron-left" : "k-i-arrow-chevron-right"
);
const selectedIndex = ref(0);
const items = computed(() => [
  {
    text: "Boards",
    icon: "k-i-set-column-position",
    selected: true,
    data: {
      path: "/",
    },
  },
  {
    text: "Templates",
    icon: "k-i-border-left",
    data: {
      path: "/templates",
    },
  },
  {
    text: "Settings",
    icon: "k-i-gear",
    data: {
      path: "/settings",
    },
  },
  {
    text: "Collapse",
    icon: expandedIcon.value,
    data: {
      action: () => (expanded.value = !expanded.value),
    },
  },
]);

function onSelect({ itemIndex }: { itemIndex: number }) {
  const item = items.value[itemIndex];

  if (item.data.path) {
    router.push(item.data.path);
    selectedIndex.value = itemIndex;
  }

  if (typeof item.data.action === "function") item.data.action();
}
</script>

<template>
  <Drawer
    class="h-[90vh]"
    :expanded="expanded"
    position="start"
    mode="push"
    :mini="true"
    :items="items.map((items, index) => ({
      ...items,
      selected: index === selectedIndex,
    }))"
    @select="onSelect"
  >
    <DrawerContent>
      <div class="px-5">
        <router-view />
      </div>
    </DrawerContent>
  </Drawer>
</template>
