<template>
  <div class="container">
    <div class="row">
      <h1 class="title">Todo-app</h1>
    </div>
    <div class="row">
      <div class="col-7">
        <div class="section">
          <div class="form-group">
            <div class="form-item col">
              <label for="inputData"><h3>What is your task?</h3></label>
              <input rows="4"
                @keyup.enter="addItem"
                id="inputData"
                v-model="newItem"
                type="text"
                placeholder="add task ..."
              >
              <br>
              <button type="button" @click="addItem" class="btn btn-submit">ADD</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <table class="mt-3 listTodo">
  <!-- <p v-if="toDos.length <= 0">Empty list</p> -->
  <tr v-for="(item, index) in toDos" :key="index" class="item-wrappers">
    <td>
      <input type="checkbox" v-model="item.checked" class="item-checkbox">
      <span class="checkmark"></span>
    </td>
    <td>
      <div class="ok">
        <span v-if="!item.editing" @click="startEdit(index)">
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
    <td width="30%" class="btn-feature" >
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
        style="width: 80.43px !important; background-color: #2ecc71;"
      >
        Edit
      </button>
      <button v-if="editingIndex === index" @click="cancelEdit(index)">
        Cancel
      </button>
      <button style="width: 80.43px !important;" v-if="editingIndex === index" @click="saveEdit(index)">
        Save
      </button>




      <!-- <button v-if="!item.checked" @click="markDone">Done</button>
      <button v-else @click="markUndone">Cancel</button> -->

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

    };
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
      this.editingIndex = index; // Đặt chỉ mục của task đang được chỉnh sửa
      this.toDos[index].editing = true;
      this.toDos[index].editedValue = this.toDos[index].value;
    },
    saveEdit(index) {
      if (this.toDos[index].editedValue.trim() !== '') {
        this.toDos[index].value = this.toDos[index].editedValue;
      }
      this.toDos[index].editing = false;
      this.editingIndex = -1; // Khi lưu xong, đặt lại chỉ mục của task đang được chỉnh sửa
    },
    cancelEdit(index) {
    this.toDos[index].editing = false;
    this.editingIndex = -1; // Đặt giá trị editingIndex về null khi cancel
    },
    markDone(index) {
      this.toDos[index].checked = true ;
      this.toDos[index].checkedValue = this.toDos[index].value;
    },
    markUndone(index) {
    this.toDos[index].checked = false;
    },

  }
};
</script>

