<template>
    <div class="flex-col flex-1 font-sans">
        <input type="text" class="inline-block border-2 border-gray-300 w-full text-lg px-5 py-1 rounded hover:border-gray-500 mb-5 focus:outline-none" 
            placeholder="What needs to be done...!" v-model="newTodo" @keyup.enter="addNewTodo">
        <!-- code to render each todo -->
        <div v-for="(todo, index) in todosFilterd" :key="todo.id" class="flex mb-4 justify-between 
            items-center text-lg text-gray-700 
            antialiased overflow-y-auto">
            <!-- checkbox code start -->
            <div class="mr-3">
              <label class="custom-label flex">
                    <div class="bg-gray-100 rounded-full border border-gray-200 shadow w-5 h-5 p-1 flex justify-center items-center mr-2">
                        <input type="checkbox" class="hidden" v-model="todo.completed">
                        <svg class="hidden w-5 h-5 text-green-600 pointer-events-none font-medium" 
                        viewBox="0 0 172 172">
                        <g fill="none" stroke-width="none" stroke-miterlimit="10" 
                        font-family="none" font-weight="none" font-size="none" 
                        text-anchor="none" style="mix-blend-mode:normal">
                        <path d="M0 172V0h172v172z"/>
                        <path d="M145.433 37.933L64.5 118.8658 33.7337 88.0996l-10.134 10.1341L64.5 139.1341l91.067-91.067z" 
                        fill="currentColor" 
                        stroke-width="1"/>
                        </g>
                        </svg>
                    </div>
                </label>
          </div>
          <!-- checkbox code ends -->
          <!-- todo item code start -->
          <div class="w-full">
              <div v-if="!todo.editing" @dblclick="editTodo(todo)" 
              class="border border-white font-medium"
              :class="{ completed: todo.completed}"
              >
                {{ todo.title }}
              </div>
              <input v-else type="text" class="inline-block border-2 border-gray-300 w-full 
                text-lg px-5 py-1 
                rounded hover:border-gray-500 
                focus:outline-none" 
                v-model="todo.title"
                @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" 
                @keyup.esc="cancelEdit(todo)"
                v-focus>
          </div>
          
          <div class="cursor-pointer mx-14 hover:text-gray-900 ml-3" @click="removeTodo(index)">
              &times;
          </div>
          <!-- todo item code ends -->
        </div>
        <!-- code ends to render each todo -->
        <!-- filter code start -->
        <div class="flex flex-1 px-3 py-2 border border-gray-300 bg-gray-100 items-center justify-between text-gray-600 font-medium">
            <div>
                <label class="custom-label flex items-center">
                    <div class="bg-gray-100 rounded-full border border-gray-200 shadow w-5 h-5 p-1 flex justify-center items-center mr-2">
                        <input type="checkbox" class="hidden" :checked="!anyRemaining" @change="CheckallTodos">
                        <svg class="hidden w-5 h-5 text-green-600 pointer-events-none font-medium" 
                        viewBox="0 0 172 172">
                        <g fill="none" stroke-width="none" stroke-miterlimit="10" 
                        font-family="none" font-weight="none" font-size="none" 
                        text-anchor="none" style="mix-blend-mode:normal">
                        <path d="M0 172V0h172v172z"/>
                        <path d="M145.433 37.933L64.5 118.8658 33.7337 88.0996l-10.134 10.1341L64.5 139.1341l91.067-91.067z" 
                        fill="currentColor" 
                        stroke-width="1"/>
                        </g>
                        </svg>
                    </div>
                    <label class="uppercase tracking-loose text-xs font-normal leading-normal">Check All</label>
                </label>
            </div>
            <div class="flex items-center">
                <button class="uppercase text-xs font-medium text-gray-600 
                focus:outline-none 
                hover:bg-green-500 
                hover:text-white 
                rounded bg-gray-200 
                border border-gray-200 
                inline-block 
                text-center px-1 py-1 mr-2"
                :class="{ 'bg-green-500 text-blue-100': filter == 'all'}" @click="filter = 'all'">
                    All
                </button>
                <button class="uppercase text-xs font-medium text-gray-600 
                focus:outline-none 
                hover:bg-green-500 
                hover:text-white 
                rounded bg-gray-200 
                border border-gray-200 
                inline-block 
                text-center px-1 py-1 mr-2"
                :class="{ 'bg-green-500 text-blue-100': filter == 'active'}" @click="filter = 'active'">
                    Active
                </button>
                <button class="uppercase text-xs font-medium text-gray-600 
                focus:outline-none 
                hover:bg-green-500 
                hover:text-white 
                rounded bg-gray-200 
                border border-gray-200 
                inline-block 
                text-center px-1 py-1 mr-2"
                :class="{ 'bg-green-500 text-blue-100': filter == 'completed'}" @click="filter = 'completed'">
                    Completed
                </button>
            </div>
            <div>
                {{ remaining }}
                <label class="uppercase tracking-loose text-xs font-normal leading-normal">Remaining Todos</label>
            </div>
        </div>
        <!-- filter code ends -->

    </div>
</template>

<script>
export default {
  name: 'TodoList',
  data (){
      return {
          newTodo: '',
          idForTodo: '4',
          beforeEditCache: '',
          remainingTodos: '',
          filter: 'all',
          emptymessage: '',                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
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
          emptyTodos: [
              {
                'id': 19999999,
                'title': 'NO TASK IS HERE!',
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
  computed: {
      remaining(){
          return this.todos.filter(todo => !todo.completed).length;
      },
      anyRemaining(){
          return this.remaining != 0;
      },
      todosFilterd(){
          if (this.filter == 'all') {
              return this.todos;
          } else if (this.filter == 'active') {
            //   if (this.todos.filter((todo) => !todo.completed).length == 0){
            //       return this.emptyTodos;
            //   }
              return this.todos.filter((todo) => !todo.completed);
          } else if (this.filter == 'completed') {
              return this.todos.filter((todo) => todo.completed);
          }

          return this.todos;
      },
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
          this.beforeEditCache = todo.title;
          todo.editing = true;
      },
      doneEdit(todo){
          if (todo.title.trim().length == 0){
              todo.title = this.beforeEditCache;
          }
          todo.editing = false;
      },
      cancelEdit(todo){
          todo.title = this.beforeEditCache;
          todo.editing = false;
      },
      CheckallTodos(){
          this.todos.forEach((todo) => todo.completed = event.target.checked);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
   .custom-label input:checked + svg {
    display: block !important;
   }

   .completed{
       text-decoration: line-through;
       color: gray;
   }

   .custom {
       background-color: green;
       color: white;
   }
</style>
