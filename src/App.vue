<script setup lang="ts">
import Card from "./components/Card.vue";
import { ref } from "vue";

const leftAreaRef = ref<HTMLDivElement>();
const rightAreaRef = ref<HTMLDivElement>();

const leftCardList = ref([...Array(5)].map((_, i) => i + 1));
const rightCardList = ref([...Array(5)].map((_, i) => i + 100));

const onMoveEndLeftCard = (
  moveCardId: number,
  movedPositionX: number,
  movedPositionY: number
) => {
  if (!rightAreaRef.value) {
    return;
  }
  const rightAreaLeftTopX = rightAreaRef.value.getBoundingClientRect().left;
  const rightAreaLeftTopY = rightAreaRef.value.getBoundingClientRect().top;
  const rightAreaRightBottomX =
    rightAreaRef.value.getBoundingClientRect().right;
  const rightAreaRightBottomY =
    rightAreaRef.value.getBoundingClientRect().bottom;

  const isIncludeX =
    rightAreaLeftTopX <= movedPositionX &&
    movedPositionX <= rightAreaRightBottomX;
  const isIncludeY =
    rightAreaLeftTopY <= movedPositionY &&
    movedPositionY <= rightAreaRightBottomY;

  if (isIncludeX && isIncludeY) {
    leftCardList.value = leftCardList.value.filter((id) => id !== moveCardId);
    rightCardList.value.push(moveCardId);
  }
};

const onMoveEndRightCard = (
  moveCardId: number,
  movedPositionX: number,
  movedPositionY: number
) => {
  if (!leftAreaRef.value) {
    return;
  }
  const leftAreaLeftTopX = leftAreaRef.value.getBoundingClientRect().left;
  const leftAreaLeftTopY = leftAreaRef.value.getBoundingClientRect().top;
  const leftAreaRightBottomX = leftAreaRef.value.getBoundingClientRect().right;
  const leftAreaRightBottomY = leftAreaRef.value.getBoundingClientRect().bottom;

  const isIncludeX =
    leftAreaLeftTopX <= movedPositionX &&
    movedPositionX <= leftAreaRightBottomX;
  const isIncludeY =
    leftAreaLeftTopY <= movedPositionY &&
    movedPositionY <= leftAreaRightBottomY;

  if (isIncludeX && isIncludeY) {
    rightCardList.value = rightCardList.value.filter((id) => id !== moveCardId);
    leftCardList.value.push(moveCardId);
  }
};
</script>

<template>
  <div class="area">
    <div class="area__left">
      <h2>領域１</h2>
      <div ref="leftAreaRef" class="container">
        <Card
          v-for="id in leftCardList"
          :key="id"
          :id="id"
          image="/src/assets/vue.svg"
          @on-mouse-up="onMoveEndLeftCard"
        />
      </div>
    </div>
    <div ref="rightAreaRef" class="area__right">
      <h2>領域２</h2>
      <div class="container">
        <Card
          v-for="id in rightCardList"
          :key="id"
          :id="id"
          image="/vite.svg"
          @on-mouse-up="onMoveEndRightCard"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
h2 {
  padding-left: 16px;
}

.area {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100%;
}

.area__left {
  background-color: darkturquoise;
  width: 50%;
  margin: 5%;
}

.area__right {
  background-color: darkseagreen;
  width: 50%;
  margin: 5%;
}

.container {
  margin: 16px;
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
</style>

