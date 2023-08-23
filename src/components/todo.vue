<template>
  <div class="container">
    <div class="row">
      <h1 class="title">Todo-app</h1>
    </div>
        <div class="section">
          <div class="form-group">
            <div class="form-item col">
              <label for="inputData"><h3>What is your task?</h3></label>
              <input rows="4"
                @keyup.enter="addItem"
                id="inputData"
                v-model="newItem"
                type="text"
                placeholder="Enter task in here..."
              >
              <br>
              <button type="button" @click="addItem" class="btn btn-submit">ADD</button>
                |  Total tasks: {{ totalTaskCount }}   |
                Completed tasks: {{ completedTaskCount }}   | 
            </div>
          </div>
        </div>
    <table class="mt-3 listTodo">
      <p v-if="toDos.length <= 0">Empty list</p>
      <tr v-for="(item, index) in toDos" :key="index" class="item-wrappers">
        <td width="5%">
          <input type="checkbox" v-model="item.checked" class="item-checkbox" @click="markDone(index)">
          <span class="checkmark"></span>
        </td>
        <td>
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
              class="edit-input"
              @keyup.enter="saveEdit(index)"
            >
          </div>
        </td>
        <td width="30%" style="text-align: right;"  >
          <div :class="{'btn-feature':item.checked}">
            <button
            v-if="!item.editing && editingIndex !== index"
            @click="deleteItem(index)"
            style="width: 84.01px; !important"
            >
            Delete
            </button>
            <button
              v-if="!item.editing && editingIndex !== index"
              @click="startEdit(index); editingIndex = index"
              style="width: 80.43px !important;
              height:36px ;
              background-color: #47d181;
              border: none;"
              >
              Edit
            </button>
            <button v-if="editingIndex === index" @click="cancelEdit(index)">
              Cancel
            </button>
            <button style="width: 80.43px !important;height:36px ; background-color: #47d181; border: none;" v-if="editingIndex === index" @click="saveEdit(index)">
              Save
            </button>
          </div>
        </td>
      </tr>
    </table>
</div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      items: [],
      toDos: [],
      editingIndex: -1,
      completedTaskCount: 0,
    };
  },
  computed: {
      totalTaskCount() {
        return this.toDos.length;
      }
    },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push({
          value: this.newItem,
          checked: false
        });
        this.toDos.push({
          value: this.newItem,
          checked: false,
          editing: false,
          editedValue: '',
          checkedValue: ''
        })
        this.newItem = ''; 
      }
    },
    deleteItem(index) {
      this.toDos.splice(index, 1); 
    },
    startEdit(index) {
      this.editingIndex = index;
      this.toDos[index].editing = true;
      this.toDos[index].editedValue = this.toDos[index].value;
    },
    saveEdit(index) {
      if (this.toDos[index].editedValue.trim() !== '') {
        this.toDos[index].value = this.toDos[index].editedValue;
      }
      this.toDos[index].editing = false;
      this.editingIndex = -1; 
    },
    cancelEdit(index) {
      this.toDos[index].editing = false;
      this.editingIndex = -1; 
    },
    markDone(index) {
      if (!this.toDos[index].checked) {
        this.toDos[index].checked = true;
        this.toDos[index].checkedValue = this.toDos[index].value;
        this.completedTaskCount++;
      }
      else {
        this.completedTaskCount--;
      }
    },
    deleteItem(index) {
      if (this.toDos[index].checked) {
        this.completedTaskCount--;
      }
      this.toDos.splice(index, 1);
    },
  }
};
</script>

