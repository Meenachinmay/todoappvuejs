<template>
    <div>
        <input type="text" class="inline-block border-2 border-gray-300 w-full text-lg px-5 py-1 rounded hover:border-gray-500 mb-5 focus:outline-none" 
            placeholder="What needs to be done...!" v-model="newTodo" @keyup.enter="addNewTodo">
        
        <div v-for="todo in todos" :key="todo.id" class="flex mb-4 justify-between items-center text-xl text-gray-700 antialiased">
          <div class="w-full">
              <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="border border-white">{{ todo.title }}</div>
              <input v-else type="text" class="inline-block border-2 border-gray-300 w-full text-lg px-5 py-1 rounded hover:border-gray-500 focus:outline-none" 
                v-model="todo.title"
                @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus>
          </div>
          
          <div class="cursor-pointer mx-14 hover:text-gray-900 ml-3" @click="removeTodo(key)">
              &times;
          </div>
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
                'completed': false,
                'editing': false
            },
            {
                'id': 2,
                'title': 'Finish vue project',
                'completed': false,
                'editing': false
            },
            {
                'id': 3,
                'title': 'Finish vue project',
                'completed': false,
                'editing': false
            }
          ],
      }
  },
  directives: {
      focus: {
          inserted: function(el){
              el.focus()
          }
      }
  },
  methods: {
      addNewTodo(){
          if (this.newTodo.trim().length == 0){
              return;
          }

          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false
          })

          this.newTodo = '';
          this.idForTodo++;
      },
      removeTodo(index){
        this.todos.splice(index, 1);
      },
      editTodo(todo){
          todo.editing = true;
      },
      doneEdit(todo){
          todo.editing = false;
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
   
</style>
