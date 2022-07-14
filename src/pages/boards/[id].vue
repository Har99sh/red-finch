<script setup lang="ts">
import { computed, toRefs } from "vue";
import AppPageHeading from "../../components/AppPageHeading.vue";
import BoardDragAndDrop from "../../components/BoardDragAndDrop.vue";
import type { Task } from "@/types";
import { v4 as uuidv4 } from "uuid";
import { useQuery } from '@vue/apollo-composable';
import boardQuery from '@/graphql/queries/board.query.gql';

const props = defineProps({
  id: String,
});

const { id: boardId } = toRefs(props);

const { result: boardData } = useQuery(
  boardQuery,
  { id: boardId?.value },
  { fetchPolicy: 'cache-and-network' }
);

const board = computed(() => boardData.value?.board || null);
const tasks = computed(() => board.value?.tasks?.items || null);

async function addTask(task: Task) {
  return new Promise((resolve, reject) => {
    const taskWithTheId = {
      ...task,
      id: uuidv4(),
    };
    tasks.value.push(taskWithTheId);
    resolve(taskWithTheId);
  });
}

const updateBoard = () => {
  // TODO: Add Update board logic
};

const deleteBoardIfConfirmed = () => {
   // @todo Delete board logic goes here
	  console.log("delete board");
};
</script>

<template>
  <div v-if="board">
    <AppPageHeading>
      {{ board.title }}
    </AppPageHeading>
    <BoardMenu :board="board" @deleteBoard="deleteBoardIfConfirmed" />

    <BoardDragAndDrop
      :tasks="tasks"
      :board="board"
      @update="updateBoard"
      :addTask="addTask"
    />
  </div>
</template>

<style scoped>
pre {
  width: 400px;
  overflow-x: auto;
  white-space: pre-wrap;
  word-wrap: break-word;
}
</style>
