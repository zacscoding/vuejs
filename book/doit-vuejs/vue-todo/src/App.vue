<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
  // EC6 import 구문 : import 불러온 파일의 내용이 담길 객체 from '불러올 파일 위치';
  import TodoHeader from "./components/TodoHeader";
  import TodoInput from "./components/TodoInput";
  import TodoList from "./components/TodoList";
  import TodoFooter from "./components/TodoFooter";

  export default {
    data() {
      return {
        todoItems: []
      }
    },
    created() {
      if (localStorage.length > 0) {
        for (var i = 0; i < localStorage.length; i++) {
          this.todoItems.push(localStorage.key(i));
        }
      }
    },
    methods   : {
      addTodo(todoItem) {
        localStorage.setItem(todoItem, todoItem);
        this.todoItems.push(todoItem);
      },
      removeTodo(todoItem, index) {
        localStorage.removeItem(todoItem);
        this.todoItems.splice(index, 1);
      },
      clearAll() {
        localStorage.clear();
        this.todoItems = [];
      }
    },
    components: {
      'TodoHeader': TodoHeader,
      'TodoInput' : TodoInput,
      'TodoList'  : TodoList,
      'TodoFooter': TodoFooter
    }
  }
</script>

<style>
  body {
    text-align: center; /* 애플리케이션 전체에서 사용하는 텍스트의 정렬 방식을 선택 */
    background-color: #F6F6F8; /* 애플리케이션 전체의 배경 색을 꾸미기 위해 지정 */
  }

  input {
    border-style: groove; /* 할 일을 입력하는 인풋 박스의 테두리 모양을 정의 */
    width: 200px;
  }

  button {
    border-style: groove;
  }

  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03) /* 할 일을 입력하는 인풋 박스와 할 일 아이템의 아래 그림자 정의 */
  }
</style>
