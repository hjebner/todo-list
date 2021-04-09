<template>

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
      <button type="submit" class="ml-3 py-1 px-3 bg-indigo-800 text-white hover:bg-white hover:text-indigo-800 text-sm uppercase">Add Todo</button>
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
                    <button type="button" @click="alertTodo(todo)" v-if="!todo.completed" class="hover:bg-red-500 hover:text-white bg-gray-400 p-1 rounded-full">
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
              <!-- Show when created and when completed -->
              <p>Created: {{todo.date}}</p>
              <!-- if is completed  -->
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

<!-- MODAL FROM https://tailwindui.com/components/application-ui/overlays/modals -->

<div v-if="alert" class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
  <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
    <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true"></div>
    <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
    <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
      <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
        <div class="sm:flex sm:items-start">
          <div class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
            <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
            </svg>
          </div>
          <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
            <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
              Delete Task {{todoSelected.text}}
            </h3>
            <div class="mt-2">
              <p class="text-sm text-gray-500">
                Sind Sie sicher, dass Sie diese Aufgabe löschen möchte.
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
        <button  @click="removeTodo(todoSelected)" type="button" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm">
          Löschen
        </button>
        <button @click="alert=false" type="button" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
          Schließen
        </button>
      </div>
    </div>
  </div>
</div>

</template>

 <script>

// Date und time pack == Link:  https://www.npmjs.com/package/vue-moment //
import moment from 'moment';

export default{

  data() {
    return{
      input:{text:""},   //Here I read the data as text
      todos:[],          // Create a array to call the To-Do 
      alert: false,      // Create an alert from Modal (False to avoid popping at loading) 
      todoSelected:null  // Not popping until I selected
    }
  },

  // Call to the localStorage
  mounted(){
    if (localStorage.getItem('todos')){
      this.todos = JSON.parse(localStorage.getItem('todos'));
    }
  },

  // Watch for any changes in To-Do's and change it if need it
  watch: {
    todos: {
      handler(){
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      deep: true,
    }
  },

  
  methods: {

    // Time and Date
    currentDate(){
      return moment().format("DD.MM.Y H:mm")
    },

    // Open Modal Alert as and give value to todoSelected
    alertTodo: function(todo) {
      this.alert = true;
      this.todoSelected = todo;
    },

    // Remove the todoSelected if Approved in the Modal
    removeTodo: function(todo) {
      this.todos.splice(todo,1)
      this.alert = false;
    },

    
    addNewTodo(){
    
      //Filter to avoid repetition
      let vm = this; // this tomar notas! 
      var exists = this.todos.filter( function(item) { return item.text.toLowerCase() === vm.input.text.toLowerCase() });

      // Count the number of words inside the todos
      //console.log(exists.length);

      // If exists is equal to 0 means that doesn't exists then we create the new Task
      if(this.input.text != '' && exists.length === 0){
          this.todos.push({text: this.input.text, date: this.currentDate(), completed_at: '', completed:false});
          this.input.text="";
      }

    },

    completedTodo(index){
      // Change todo to status
      index.completed = !index.completed;
      
      // If is Completed give completed_at value to know when was completed
      if(index.completed){
        index.completed_at = this.currentDate();
      }else{
        index.completed_at = '';
      }
    }
  }
}
 </script>
