<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @childAddTodo="addTodo" @noInputValue="toggleModal"></TodoInput>
    <TodoList v-bind:propsItems="todoItems" @childRemoveTodo="removeTodo"></TodoList>
    <TodoFooter @clearTodo="toggleModal"></TodoFooter>
  </div>
  <AlertModal @close="toggleModal" :show="modalShow" header="알림창" body="내용을 입력해 주세요"></AlertModal>
</template>

<script>
import TodoHeader from "./components/todo/TodoHeader.vue";
import TodoInput from "./components/todo/TodoInput.vue";
import TodoList from "./components/todo/TodoList.vue";
import TodoFooter from "./components/todo/TodoFooter.vue";
import AlertModal from "./components/common/AlertModal.vue";

export default {
  name: "App",
  data() {
    return {
      todoItems: [],
      cnt: 0,
      modalShow: false,
    };
  },
  methods: {
    toggleModal() {
      this.modalShow = !this.modalShow;
    },
    addTodo(todoItem) {
      // localStorage.setItem(todoItem, todoItem);
      this.todoItems.push({
        key: this.cnt++,
        value: todoItem,
      });
      // this.changeValue();
      //watch라는 속성을 사용해서 윗줄 삭제
    },
    removeTodo(key) {
      // localStorage.removeItem(todoItem);
      // this.todoItems.splice(idx, 1);
      this.todoItems.forEach((item, idx) => {
        if (item.key === key) {
          this.todoItems.splice(idx, 1);
        }
      });

      // this.changeValue();
    },
    clearTodo() {
      this.todoItems.splice(0);
      this.cnt = 0;
      // idx값 0넣으면 모두삭제..
      // localStorage.clear();
      // this.changeValue();
    },
    changeValue() {
      const json = JSON.stringify(this.todoItems);
      localStorage.setItem("todoItems", json);
    },
  },
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
    AlertModal,
  },
  watch: {
    todoItems: {
      deep: true,
      //배열안의 값이 바뀌는지 체크(배열이 아니라 원시타입변수라면 딥속성없어도됨)
      // = 레퍼런스타입은 deep을줘야하고 원시타입한테는 안줘도된다 ///근데 무조건줘도 동작하는데 속도차이가 있을듯
      handler() {
        this.changeValue();
      },
    },
  },
  created() {
    const json = localStorage.getItem("todoItems");
    if (json) {
      const todoItems = JSON.parse(json);
      todoItems.forEach((item) => {
        this.todoItems.push(item);
      });
      const cnt = localStorage.getItem("cnt");
      this.cnt = cnt;
    }
    // if (localStorage.length > 0) {
    //   for (let i = 0; i < localStorage.length; i++) {
    //     this.todoItems.push(localStorage.key(i));
    //   }
    // }
  },
};
//created 부분은 처음실행시에 한번 실행되는부분.
</script>

<style>
body {
  text-align: center;
  background-color: #f6f6f8;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
