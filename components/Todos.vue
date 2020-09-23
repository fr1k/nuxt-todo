<template>
  <div>
    <b-card header="Todos App" deck class="card">
      <b-form-textarea
        placeholder="Enter something..."
        rows="3"
        max-rows="3"
        size="sm"
        v-model="textItem"
        @keydown.enter="addTodo"
        class="mb"
      ></b-form-textarea>
      <b-list-group class="mb">
        <b-list-group-item
          class="item"
          v-for="(item, index) in todoItems"
          :key="index"
          :class="{ 'line-through': item.done }"
        >
          <b-icon
            :icon="item.done ? 'check-square' : 'square'"
            class="cp mr-3"
            scale="1.2"
            @click="done(index)"
          ></b-icon>
          <span
            class="cp list-item"
            @click="editTodo(index)"
            :changeTodo="changeTodo"
          >
            {{ item.name }}
          </span>
          <b-icon
            icon="trash"
            class="cp ml-3"
            scale="1.2"
            @click="delTodo(index)"
          ></b-icon>
          <b-modal
            :id="index.toString()"
            :ref="index.toString()"
            hide-footer
            title="Edit todo..."
          >
            <Modal :text="item.name" />
          </b-modal>
        </b-list-group-item>
      </b-list-group>
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      textItem: "",
      todoItems: [],
      isLoad: true,
      modalText: ""
    };
  },
  methods: {
    addTodo() {
      if (this.textItem.length) {
        const obj = {
          id: this.todoItems.length + 1,
          name: this.textItem,
          done: false
        };
        this.todoItems.push(obj);
        this.textItem = "";
      }
    },
    delTodo(id) {
      this.todoItems.splice(id, 1);
    },
    done(id) {
      this.todoItems[id].done = !this.todoItems[id].done;
    },
    editTodo(id) {
      this.$bvModal.show(id.toString());
      // this.$refs[id.toString()].show();
    },
    changeTodo(todoItem) {
      this.modalText = todoItem;
    }
  },
  mounted() {
    if (this.isLoad) {
      this.todoItems = JSON.parse(localStorage.getItem("todos"));
      this.isLoad = false;
    }
  },
  watch: {
    todoItems() {
      localStorage.setItem("todos", JSON.stringify(this.todoItems));
    }
  }
};
</script>

<style lang="scss" scoped>
.card {
  min-width: 200px;
  .line-through {
    text-decoration: line-through;
  }
  .item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    .list-item {
      font-size: 1.5rem;
    }
  }
}
</style>
