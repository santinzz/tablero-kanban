<script setup>
import { reactive, ref } from 'vue';
import Input from './components/Input.vue';

const isBoardCreatorClicked = ref(false);

let boards = reactive([
  {
    id: crypto.randomUUID(),
    name: 'Board 1',
    items: [
      {
        id: crypto.randomUUID(),
        title: 'Item 1',
      },
      {
        id: crypto.randomUUID(),
        title: 'Item 2',
      }
    ]
  },
  {
    id: crypto.randomUUID(),
    name: 'Board 2',
    items: [
      {
        id: crypto.randomUUID(),
        title: 'Item 3',
      },
      {
        id: crypto.randomUUID(),
        title: 'Item 4',
      }
    ]
  },
  {
    id: crypto.randomUUID(),
    name: 'Board 3',
    items: [
      {
        id: crypto.randomUUID(),
        title: 'Item 5',
      },
      {
        id: crypto.randomUUID(),
        title: 'Item 6',
      }
    ]
  }
]);

const handleNewItem = (text, board) => {
  board.items.push({
    id: crypto.randomUUID(),
    title: text
  })
}

const handleNewBoard = () => {
  isBoardCreatorClicked.value = !isBoardCreatorClicked.value;
}

const createBoard = (boardName) => {
  boards.push({
    id: crypto.randomUUID(),
    name: boardName,
    items: []
  });
  isBoardCreatorClicked.value = false;
}

const startDrag = (event, board, item) => {
  event
    .dataTransfer
    .setData(
      'text/plain',
      JSON.stringify({ boardId: board.id, itemId: item.id })
    );
}

const onDrop = (event, destinationBoard) => {
  const { boardId, itemId } = JSON.parse(event.dataTransfer.getData('text/plain'));

  const board = boards.find(board => board.id === boardId);
  const item = board.items.find(item => item.id === itemId);

  board.items = board.items.filter(item => item.id !== itemId);

  destinationBoard.items.push(item);
}
</script>

<template>
  <div>
    <nav>
      <button class="add-board-button" @click="handleNewBoard">
        Add board
      </button>
    </nav>

    <div class="board-creator" v-show="isBoardCreatorClicked">
      <span>Create a new board</span>
      <Input v-if="isBoardCreatorClicked" @on-new-item="createBoard" />
    </div>

    <main class="boards-container">
      <div class="boards">
        <div class="board" v-for="board in boards" :key="board.id" @drop="onDrop($event, board)" @dragover.prevent
          @dragenter.prevent>
          <h3>
            {{ board.name }}
          </h3>
          <Input @on-new-item="(text) => handleNewItem(text, board)" />
          <ul v-if="board.items.length > 0">
            <li class="item" v-for="item in board.items" :key="item.id" draggable="true"
              @dragstart="startDrag($event, board, item)">
              {{ item.title }}
            </li>
          </ul>
          <p v-else style="padding-top: 16px;">
            No items
          </p>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
nav {
  padding: 20px;
}

ul {
  list-style: none;
  padding: 0;
}

.board-creator {
  position: absolute;
  top: 0;
  left: 0;
  margin: 6px 160px;
  border-radius: 8px;
  background-color: #fff;
  border: 1px solid #000;
  padding: 8px 12px;
}

.boards {
  display: grid;
  grid-template-columns:
    repeat(auto-fill,
      minmax(250px, 1fr));
  gap: 16px;
}

.add-board-button {
  padding: 8px;
  border: none;
  border-radius: 8px;
  background-color: #536da3;
  color: white;
  cursor: pointer;
  font-size: 1.2em;
}

.board {
  padding: 16px;
  border: 1px solid #000;
  border-radius: 8px;
  background-color: #f0f0f0;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
  min-width: 200px;
  height: fit-content;
}


.item {
  padding: 8px;
  border: 1px solid #000;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
  margin-top: 8px;
}
</style>
