<template>
<div class="flex justify-center">
  <div class="text-center uppercase">
<h1><a href="/src/components/TodoList.vue">Deutsch</a></h1>
</div>
</div>

<!-- Here I make the box be able to flex -->
<div class="flex">
  <div class="flex-none w-1/6">
  </div>

<!-- Style to the Box Border-->

  <div class="flex-grow w-4/6 border-2 border-blue-900 rounded ">
     
      <!-- To-Do Title -->
     
     <div class="bg-blue-700  text-white text-center uppercase font-bold">List of Todos</div>
  
  <!-- To-Do Input&Button -->

  <div class="flex justify-center bg-indigo-300 py-2">
    <form v-on:submit.prevent="addNewTodo">
      <input type="text" v-model="input.text" class="py-0.5 text-center border-indigo-900 border focus:ring-100">
      <button type="submit" class="ml-3 py-1 px-3 bg-indigo-800 text-white hover:bg-white hover:text-indigo-800 text-sm uppercase" @click="panel=true">Add Todo</button>
    </form>
  </div>

  <!-- Table Header -->
  <!-- Ref: https://tailwindui.com/components/application-ui/lists/tables // Used as Guide and later on by my on change it -->

<table class="min-w-full divide-y divide-gray-200">
          <thead class="bg-gray-50">
            <tr>
              <th scope="col" class="px-3 py-3 text-left text-xs font-medium text-gray-50 uppercase tracking-wider">
                
              </th>
              <th scope="col" class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">
                Date
              </th>
              <th scope="col" class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">
                Task
              </th>
              <th scope="col" class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">
                Status
              </th>
            </tr>
          </thead>
           
           <!-- Table Body = Written Input // Check-Mark -->

          <tbody class="bg-white divide-y divide-gray-200">
            <tr v-for="(todo, index) in todos" :class="[todo.completed?'bg-green-300':'bg-purple-200']">
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="flex items-center">
                  <div class="ml-4">
                    <div class="text-sm font-medium text-gray-900">
                       
           <!-- Remove Botton / Icon + Style-->
          
                       <button type="button" @click="removeTodo(index)" v-if="!todo.completed" class="hover:bg-red-500 hover:text-white bg-gray-400 p-1 rounded-full">
                          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                             <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /> 
                          </svg>
                       </button>
                    </div>
                  </div>
                </div>
              </td>
              
            <!-- Date, Time and Status -->
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="text-xs text-gray-900 text-left">
                  <p>Created: {{todo.date}}</p>
                  <p v-if="todo.completed">Completed: {{todo.completed_at}}</p>
                </div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-center" v-text="todo.text">
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-center">
                <input type="checkbox" @click="completedTodo(todo)">
              </td>
            </tr>
          </tbody>
        </table>
  </div>

            <!-- Centering box -->

          <div class="flex-none w-1/6">
          </div>
        </div>

        <div class="flex flex-col justify-center h-screen w-4/6">
        
        </div>

</template>

 <script>

          // Date und time pack == Link:  https://www.npmjs.com/package/vue-moment //

import moment from 'moment';
export default{

  data() {
    return{
      input:{text:""},
      todos:[]
    }
  },

// Call to the localStorage
  
  mounted(){
    if (localStorage.getItem('todos')){
      this.todos = JSON.parse(localStorage.getItem('todos'));
    }
  },

  watch: {
    todos: {
      handler(){
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      deep: true,
    }
  },

// Time and Date Germany
  
  methods: {
    currentDate(){
      moment.locale('DE')
      return moment().format("DD.MM.Y H:mm")
    },

    removeTodo: function(index) {
      this.todos.splice(index,1)
    },
    
    
    //Filter to avoid repetition


    addNewTodo(){
      let vm = this; // this tomar notas! 
      var exists = this.todos.filter( function(item) { return item.text.toLowerCase() === vm.input.text.toLowerCase() });
      console.log(exists.length);

      if(this.input.text != '' && exists.length === 0){
          this.todos.push({text: this.input.text, date: this.currentDate(), completed_at: '', completed:false});
          this.input.text="";
      }

    },

    completedTodo(index){
      index.completed = !index.completed;
      index.open = !index.open;
      if(index.completed){
        index.completed_at = this.currentDate();
      }else{
        index.completed_at = '';
      }
    }
  }
}
 </script>
