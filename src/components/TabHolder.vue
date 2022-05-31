//abhishek360

<script lang="ts">
  import {
    Card,
  } from 'ant-design-vue';
  import { defineComponent, ref, computed } from 'vue';
  import TabItem from "./TabItem.vue";

  export default defineComponent({
    name: 'TabHolder',
    components: {
      TabItem,
    },
    setup() {
      const panes = ref([
        { title: 'Abhishek', content: 'Content of Tab Abhi' },
        { title: 'Ali', content: 'Content of Tab Ali'},
        { title: 'Supratim', content: 'Content of Tab Supratim'},
        { title: 'Ritik', content: 'Content of Tab Ritik'},
      ]);

      const activeKey = ref(0);
      const newTabIndex = ref(0);
      
      const callback = (key: string) => {
        console.log(key);
      };

      const add = () => {
        activeKey.value = `newTab${++newTabIndex.value}`;
        panes.value.push({ title: 'New Tab', content: 'Content of new Tab', key: activeKey.value });
      };

      const remove = (targetKey: string) => {
        let lastIndex = 0;
        
        panes.value.forEach((pane, i) => {
          if (pane.key === targetKey) {
            lastIndex = i - 1;
          }
        });

        panes.value = panes.value.filter(pane => pane.key !== targetKey);
        
        if (panes.value.length && activeKey.value === targetKey) {
          
          if (lastIndex >= 0) {
            activeKey.value = panes.value[lastIndex].key;
          } else {
            activeKey.value = panes.value[0].key;
          }
        }
      };

      const onEdit = (targetKey: string | MouseEvent, action: string) => {
        if (action === 'add') {
          add();
        } else {
          remove(targetKey as string);
        }
      };

      const sortTabsOnDrop = (currentKey: int, dropKey: int) => {
        console.log("Current Key: " + currentKey + " Drop Key: " + dropKey)

        const newPanes = panes.value.slice();
        const content = panes.value[currentKey];

        newPanes.splice(currentKey, 1);

        newPanes.splice(dropKey, 0, content);

        panes.value=newPanes

        if(activeKey==currentKey)
          activeKey.value=dropKey

        if(dropKey==activeKey.value)
          activeKey.value=dropKey+1
      }

      return {
        panes,
        activeKey,
        callback,
        onEdit,
        sortTabsOnDrop
      };
    },
  });
</script>

<template>
  <a-tabs v-model:activeKey="activeKey" type="editable-card" @edit="onEdit">
      <a-tab-pane 
        v-for="(pane, index) in panes" 
        :key="index" 
        :closable="pane.closable"
        class="ant-tabs-tab"
      >
        <template #tab>
          <TabItem :title="pane.title" @onDroped="sortTabsOnDrop" :index="index"/>
        </template>
        {{ pane.content }}
      </a-tab-pane>
      <template #renderTabBar="{ DefaultTabBar, ...props }">
        <component :is="DefaultTabBar" v-bind="props" :style="{ color: 'rgb(229, 233, 236)'}"/>
      </template>
  </a-tabs>
</template>

<style scoped>
  .ant-tabs-tab {
    margin: 0;
    padding: 0;
  }
</style>