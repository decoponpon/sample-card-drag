<script setup lang="ts">
import { useDraggable } from "@vueuse/core";
import { onMounted, ref } from "vue";

const props = defineProps<{
  id: number;
  image: string;
}>();

const emits = defineEmits<{
  onMouseUp: [id: number, positionX: number, positionY: number];
}>();

const cardRef = ref<HTMLDivElement>();
const imageRef = ref<HTMLImageElement>();

const { x, y, style } = useDraggable(cardRef, {
  initialValue: { x: 40, y: 40 },
});

const onDragStart = () => {
  if (!cardRef.value) {
    return;
  }
  cardRef.value.style.position = "fixed";
  cardRef.value.style.cursor = "grabbing";
};

const onMouseUp = (event: MouseEvent) => {
  if (!cardRef.value) {
    return;
  }
  emits("onMouseUp", props.id, event.pageX, event.pageY);
  cardRef.value.style.position = "";
  cardRef.value.style.cursor = "grab";
};

onMounted(() => {
  if (imageRef.value) {
    imageRef.value.ondragstart = () => {
      return false;
    };
  }
});
</script>

<template>
  <div
    ref="cardRef"
    :style="style"
    style=""
    class="card"
    @dragstart="onDragStart"
    @mouseup="onMouseUp"
  >
    id: {{ id }}
    <img ref="imageRef" :src="image" alt="Card image" class="card-img" />
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
  cursor: grab;
}

.card:hover {
  transform: scale(1.05);
}

.card-img {
  width: 100%;
  height: 100px;
}
</style>
