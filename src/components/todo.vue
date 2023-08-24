<template>
  <div class="container">
    <div class="row">
      <h1 class="title">Todo-app</h1>
    </div>
    <div class="section">
      <div class="form-group">
        <div class="form-item col">
            <h2>What is your task?</h2>
          <div class="quantity">
            Total tasks: <p> {{ totalTaskCount }} </p>  | Completed tasks: <p>{{ completedTaskCount }}</p>
          </div>
          <input
            rows="4"
            @keyup.enter="addItem"
            id="inputData"
            v-model="newItem"
            type="text"
            placeholder="Enter task in here..."
          >
          <br>
          <button type="button" @click="addItem" class="btn btn-submit">ADD</button>
        </div>
      </div>
    </div>
    <div  class="listTodo" v-for="(item, index) in toDos" :key="index" >
      <div class="item-wrappers">
        <div class="checkbox-wrapper">
          <input type="checkbox" v-model="item.checked" class="item-checkbox" @click="markDone(index)">
          <span class="checkmark"></span>
        </div>
        <div class="task-wrapper">
          <div class="ok">
            <span
              v-if="!item.editing"
              @click="startEdit(index)"
              :class="{ 'completed-task': item.checked }"
            >
              {{ item.value }}
            </span>
            <input
              rows="5"
              v-else
              v-model="item.editedValue"
              :class="{'disable': item.checked}"
              class="edit-input"
              @keyup.enter="saveEdit(index)"
              :disabled="checkboxChecked || item.checked"
            >
          </div>
        </div>
        <div class="buttons-wrapper">
          <div :class="{'btn-feature':item.checked}" class="edit-buttons">
            <button
              class="edit-button" title="Edit"
              v-if="!item.editing && !editingIndex[index]"
              @click="startEdit(index)"
              :disabled="checkboxChecked || item.checked"
            >
            <svg style="fill:#ff7e7e" xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 512 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M471.6 21.7c-21.9-21.9-57.3-21.9-79.2 0L362.3 51.7l97.9 97.9 30.1-30.1c21.9-21.9 21.9-57.3 0-79.2L471.6 21.7zm-299.2 220c-6.1 6.1-10.8 13.6-13.5 21.9l-29.6 88.8c-2.9 8.6-.6 18.1 5.8 24.6s15.9 8.7 24.6 5.8l88.8-29.6c8.2-2.7 15.7-7.4 21.9-13.5L437.7 172.3 339.7 74.3 172.4 241.7zM96 64C43 64 0 107 0 160V416c0 53 43 96 96 96H352c53 0 96-43 96-96V320c0-17.7-14.3-32-32-32s-32 14.3-32 32v96c0 17.7-14.3 32-32 32H96c-17.7 0-32-14.3-32-32V160c0-17.7 14.3-32 32-32h96c17.7 0 32-14.3 32-32s-14.3-32-32-32H96z"/></svg>            
            </button>
            <button
              class="delete-button" title="Delete"
              v-if="!item.editing && !editingIndex[index]"
              @click="deleteItem(index)"
              :disabled="checkboxChecked || item.checked"
            >
            <svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 448 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M135.2 17.7C140.6 6.8 151.7 0 163.8 0H284.2c12.1 0 23.2 6.8 28.6 17.7L320 32h96c17.7 0 32 14.3 32 32s-14.3 32-32 32H32C14.3 96 0 81.7 0 64S14.3 32 32 32h96l7.2-14.3zM32 128H416V448c0 35.3-28.7 64-64 64H96c-35.3 0-64-28.7-64-64V128zm96 64c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16z"/></svg>            
            </button>
            <button
              class="save-button" title="Save"
              v-show="editingIndex[index]"
              @click="saveEdit(index)"
              :disabled="checkboxChecked || item.checked"       
              >
              <svg style="fill:#ff6e6e" xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 448 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M438.6 105.4c12.5 12.5 12.5 32.8 0 45.3l-256 256c-12.5 12.5-32.8 12.5-45.3 0l-128-128c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0L160 338.7 393.4 105.4c12.5-12.5 32.8-12.5 45.3 0z"/></svg>
            </button>
            <button
              class="cancel-button" title="cancel"
              v-show="editingIndex[index]"
              @click="cancelEdit(index)"
              :disabled="checkboxChecked || item.checked"
            >
            <svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 384 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z"/></svg>            
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      newItem: '',
      items: [],
      toDos: [],
      editingIndex: [],
      completedTaskCount: 0,
      checkboxChecked: false,
    };
  },
  computed: {
    totalTaskCount() {
      return this.toDos.length;
    },
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push({
          value: this.newItem,
          checked: false,
        });
        this.toDos.push({
          value: this.newItem,
          checked: false,
          editing: false,
          editedValue: '',
          checkedValue: '',
        });
        this.newItem = '';
      }
    },

    startEdit(index) {
      this.editingIndex[index] = true;
      this.toDos[index].editing = true;
      this.toDos[index].editedValue = this.toDos[index].value;
    },
    saveEdit(index) {
      if (this.toDos[index].editedValue.trim() !== '') {
        this.toDos[index].value = this.toDos[index].editedValue;
      }
      this.toDos[index].editing = false;
      this.editingIndex[index] = false;
    },
    cancelEdit(index) {
      this.toDos[index].editing = false;
      this.editingIndex[index] = false;
    },
    markDone(index) {
      if (!this.toDos[index].checked) {
        this.completedTaskCount++;
        if (this.editingIndex[index]) {
          this.editingIndex[index] = false;
          this.toDos[index].editing = false;
        }
      } else {
        this.completedTaskCount--;
        this.checkboxChecked = false;
      }
    },
    deleteItem(index) {
      this.toDos.splice(index, 1);
      this.editingIndex[index] = true;
      this.editingIndex.splice(index, 1);
    },
  },
};
</script>
