<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" placeholder="Type what you have to do"
           v-on:keyup.enter="addTodo">
    <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"></i>
    </span>

    <modal v-if="showModal" @close="showModal = false">
      <h3 slot="header">경고</h3>
      <span slot="footer" @click="showModal = false">할 일을 입력하세요.
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
    </modal>
  </div>
</template>

<script>
  import Modal from './common/Modal.vue'

  export default {
    data() {
      return {
        newTodoItem: '',
        showModal  : false
      }
    },
    methods   : {
      addTodo() {
        if (this.newTodoItem !== "") {
          let value = this.newTodoItem && this.newTodoItem.trim();
          this.$emit('addTodo', value);
          this.clearInput();
        } else {
          this.showModal = !this.showModal;
        }
      },
      clearInput() {
        this.newTodoItem = '';
      }
    },
    components: {
      Modal: Modal
    }
  }
</script>

<style scoped>
  input:focus {
    outline: none; /* 할 일을 입력하는 인풋 박스의 선 스타일 지정 */
  }

  .inputBox {
    background: white; /* 인풋 박스의 배경색 지정 */
    height: 50px; /* 인풋 박스의 높이 설정*/
    line-height: 50px; /* 인풋 박스에 입력되는 텍스트의 중앙 정렬을 위해 설정 */
    border-radius: 5px; /* 인풋 박스의 둥근 테두리 속성 설정 */
  }

  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }

  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: inline-block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }

  .addBtn {
    color: white;
    vertical-align: middle;
  }
</style>
