<script setup lang="ts">
const leftItems = ref([
  {
    label: 'Home',
    icon: 'pi pi-home',
  },
  {
    label: 'Agents',
    icon: 'pi pi-users',
  },
  {
    label: 'Explore',
    icon: 'pi pi-search',
  },
  {
    separator: true,
  },
  
  {
    label: 'Settings',
    icon: 'pi pi-cog',
  }
])
</script>

<template>
  <div class="w-screen h-screen p-5">
    <div class="flex flex-row h-full">
      <div class="card flex flex-grow-[1] h-full">
        <TieredMenu :model="leftItems" class="w-full h-full"/>
      </div>
      <div class="card flex flex-grow-[9] h-full">
        <slot></slot>
      </div>
    </div>
  </div>
</template>