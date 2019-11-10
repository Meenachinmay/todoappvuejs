<template>
    <div>
        <input type="text" class="todo-input" placeholder="What needs to be done...!" v-model="newTodo" @keyup.enter="addNewTodo">
        <div v-for="todo in todos" :key="todo.id" class="todo-item">
            {{ todo.title }}
        </div>
    </div>
</template>

<script>
export default {
  name: 'TodoList',
  data (){
      return {
          newTodo: '',
          idForTodo: '4',
          todos: [
            {
                'id': 1,
                'title': 'Finish vue project',
                'completed': false
            },
            {
                'id': 2,
                'title': 'Finish vue project',
                'completed': false
            },
            {
                'id': 3,
                'title': 'Finish vue project',
                'completed': false
            }
          ],
      }
  },
  methods: {
      addNewTodo(){
          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false
          })

          this.newTodo = '';
          this.idForTodo++;
      }
  }
}
</script>a

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    .todo-input{
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
        
        &:focus {
            outline: 0px;
        }
    }
</style>
