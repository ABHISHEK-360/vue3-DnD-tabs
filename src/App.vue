<template>
  <div>Tab View Drag and Drop</div>
  <DndProvider :backend="HTML5Backend">
    <TabHolder></TabHolder>
  </DndProvider>
</template>

<script lang="ts" setup>
  import {defineComponent} from 'vue';
  import TabHolder from './components/TabHolder.vue'
  import TabItem from './components/TabItem.vue'
  import SimpleDragAndDrop from './components/SimpleDragAndDrop.vue';
  import { DndProvider } from 'vue3-dnd'
  import { HTML5Backend } from 'react-dnd-html5-backend'

  // export default defineComponent({
  //   name: 'App',

  //   setup() {
  //     return {
  //       TabHolder,
  //       TabItem,
  //       SimpleDragAndDrop,
  //       DndProvider,
  //       HTML5Backend
  //     }
  //   }
  // })
</script>
