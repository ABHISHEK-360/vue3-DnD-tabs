
<script lang="ts">
  import { useDrag, useDrop } from 'vue3-dnd'
  import { computed, unref, defineComponent } from 'vue'

  export default defineComponent({
    name: "TabItem",
    props: ['title', 'index'],
    setup(props, context) {
      const [dropCollect, drop] = useDrop(() => ({
        accept: 'Box',
        drop: () => ({ 
          name: props.title,
          index: props.index 
        }),
        collect: monitor => ({
          isOver: monitor.isOver(),
          canDrop: monitor.canDrop(),
        }),
      }))

      const canDrop = computed(() => unref(dropCollect).canDrop)
      const isOver = computed(() => unref(dropCollect).isOver)
      const isActive = computed(() => unref(canDrop) && unref(isOver))

      const [collect, drag] = useDrag(() => ({
        type: 'Box',
        item: () => ({
            name: props.title,
            index: props.index
        }),
        end: (item, monitor) => {
            const dropResult = monitor.getDropResult<{ name: string }>()
            if (item && dropResult) {
                context.emit('onDroped', item.index, dropResult.index)
                console.log(`You dropped ${item.index} into ${dropResult.index}!`)
            }
        },
        collect: monitor => ({
            isDragging: monitor.isDragging(),
            handlerId: monitor.getHandlerId(),
        }),
      }))
    
      const isDragging = computed(() => collect.value.isDragging)

      const opacity = computed(() => (unref(isDragging) ? 0.4 : 1))

      return {
        drag,
        drop,
        isActive,
        isOver,
        isDragging
      }
    }
  })
</script>

<template>
  <div 
    :ref="drop"
    role="Box"
  >
    <div 
      :ref="drag" 
      :class="{box: true, 'box-dragging': isDragging, 'box-hover': isOver}"
      role="Box"
    >
      {{title}}
    </div>
  </div>
</template>

<style scoped>
  .drop-container {
    color: white;
    text-align: center;
    float: left;
  }
  .box {
    background-color: white;
    color: black;
    padding: 0.5rem 1rem;
    cursor: move;
    float: left;
  }
  .box-dragging {
    background-color: white;
    color: rgb(124, 119, 185);
  }
  .box-hover {
    background-color: rgb(244, 246, 253);
    color: grey;
    padding: 0.5rem 1rem;
    cursor: move;
    float: left;
    &.dragging {
      border: 1px solid gray;
    }
  }
</style>