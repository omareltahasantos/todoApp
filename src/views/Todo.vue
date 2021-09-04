<template>
  <div class="home ">
    <v-text-field
            v-model="newTaskTitle"
            class="pa-3"
            outlined
            label="Append"
            append-icon="mdi-plus"
            hide-details
            clearable
            @keypress.enter="addTask()"
            @click:append ="addTask() "
           

    ></v-text-field>
      <v-row no-gutters class="text-center">
        <v-col
           
           
            cols="3"
            sm="4"
            md="4"
          >
            <v-btn v-if="tasks !== [] " outlined class="ml-2" @click="orderTask()">
              <p class="upButton pt-4 mr-1">Sort </p>
              <v-icon v-if="sortIcon === true" color="blue lighten-1">mdi-sort-bool-ascending-variant</v-icon>
              <v-icon v-if="sortIcon === false" color="blue lighten-1">mdi-sort-bool-descending-variant</v-icon>
            </v-btn>
       </v-col>
          <v-col
           
           
            cols="5"
            sm="4"
            md="4"
          >
          <v-btn v-if="tasks !== [] " outlined class="ml-3" @click="deleteAllTask()">
              <p class="upButton pt-4 mr-1">Delete All</p>
              
            </v-btn>
       </v-col>
          <v-col
            cols="4"
            sm="4"
            md="4"
          >
            <v-btn v-if="tasks !== [] " outlined class="ml-1"  @click="deleteDoneTask()">
              <p class="upButton pt-4 mr-1">Delete done</p>
            </v-btn>
       </v-col>
      </v-row>
     <v-list class="pt-0"
      flat
    >
      
       <div v-for="task,id in tasks" :key="id">
         <v-list-item @click="doneTask(task.id)" :class="{'blue lighten-5' : task.done}"  >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title :class="{'text-decoration-line-through' : task.done}">{{task.title}}</v-list-item-title>
              <v-list-item-subtitle></v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action @click.stop="deleteTask(task.id)">
                   <v-menu>
                    <template v-slot:activator="{ on: menu, attrs }">
                      <v-tooltip bottom>
                        <template v-slot:activator="{  }">
                          <v-btn 
                            icon
                            color="primary"
                            dark
                            v-bind="attrs"
                            v-on="{ ...menu }"
                          >
                            <v-icon color="blue lighten-1">mdi-dots-vertical</v-icon>
                          </v-btn>
                        </template>
                      </v-tooltip>
                    </template>
                    <v-list class="pt-0 pb-0 " flat dense>
                      <v-list-item >
                         <v-list-item-content>
                            <v-list-item-title @click="deleteTask(task.id)">
                                <v-btn
                                icon
                                >
                                  <v-icon color="red lighten-1">mdi-delete</v-icon>
                                </v-btn>
                              </v-list-item-title>
                          </v-list-item-content>
                      
                          <v-list-item-action ml-0>
                              Delete
                          </v-list-item-action>
                      </v-list-item>
                    </v-list>
                     <v-list class="pt-0 pb-0" flat dense>
                      <v-list-item >
                         <v-list-item-content>
                            <v-list-item-title @click.stop="editButton(task.id) ">
                                <v-btn
                                icon
                                >
                                  <v-icon color="blue lighten-1">mdi-pencil</v-icon>
                                </v-btn>
                              </v-list-item-title>
                          </v-list-item-content>
                      
                          <v-list-item-action ml-0>
                              Edit
                          </v-list-item-action>
                      </v-list-item>
                    </v-list>
                    <v-dialog
                        v-model="dialog"
                        max-width="290"
                      >
                          <v-card>
                            <v-card-title class="text-h5">
                            Edit task
                            </v-card-title>

                            <v-card-subtitle class="pt-5">
                              Edit the title of this task:
                            </v-card-subtitle>

                              <v-card-text>
                                <v-text-field
                                v-model="editTaskTitle"
                                ></v-text-field>
                              </v-card-text>
                            <v-card-actions>
                              <v-spacer></v-spacer>

                              <v-btn
                                color=" darken-1"
                                text
                                @click="dialog = false"
                              >
                                Cancel
                              </v-btn>

                              <v-btn
                                color="red darken-1"
                                text
                                @click="changeTaskTitle(task.id)"
                              >
                                Save
                              </v-btn>
                            </v-card-actions>
                          </v-card>
                     </v-dialog>
                   
                   </v-menu>
              </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>

      
    <!-- -->
     
      


   
     
    </v-list>
   <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-snackbar
      v-model="snackbarDelete"
      :multi-line="multiLineDelete"
      
    >
      {{ textDelete }}

      <template v-slot:action="{ attrs2}">
        <v-btn
          color="red"
          text
          v-bind="attrs2"
          @click="snackbarDelete = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <!-- Snack para boton delete All --> 
     <v-snackbar
      v-model="snackbarDeleteAll"
      :multi-line="multiLineDeleteAll"
      
    >
      {{ textDeleteAll }}

      <template v-slot:action="{ attrs3}">
        <v-btn
          color="red"
          text
          v-bind="attrs3"
          @click="snackbarDeleteAll = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <!-- Snack para boton delete by done task --> 
     <v-snackbar
      v-model="snackbarDeleteByDone"
      :multi-line="multiLineDeleteByDone"
      
    >
      {{ textDeleteByDone }}

      <template v-slot:action="{ attrs4}">
        <v-btn
          color="red"
          text
          v-bind="attrs4"
          @click="snackbarDeleteByDone = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar> 
  </div>
</template>

<script>
  import HelloWorld from '../components/HelloWorld'

  export default {
    name: 'Todo',

    components: {
      HelloWorld,
    },

    data(){
      return{
        sortIcon: false,
        dialog: false,
        editTaskTitle: '',
        newTaskTitle: '',
        taskEmpty: '',
        NewArray: '',
        multiLine: true,
         snackbar: false,
         multiLineDelete: true,
         snackbarDelete: false,
          snackbarDeleteAll: false,
        multiLineDeleteAll: false,
        snackbarDeleteByDone: false,
        multiLineDeleteByDone: false,
          textDelete: `Task deleted!`,
          textDeleteAll: 'There is no task for delete!',
          textDeleteByDone: 'There is no task done!',
          text: `Task added!`,
        tasks: [
          
          {id: 1, title: 'Do the homework', done: false},
          {id: 2, title: 'Study English', done: false},
          {id: 3, title: 'Eat bananas', done: false},

        
        ],
        contador: 0
      }
    },
       created(){
        if(this.tasks === ''){
          this.taskEmpty = 'There is no task'
        }
      },
    methods: {
  
      addTask(){

        if(this.newTaskTitle !== ''){
          this.tasks.push({
          id: this.contador++,
          title: this.newTaskTitle,
          done: false,
        })

        this.snackbar = true
        }
        
        this.newTaskTitle = ''
      },
      doneTask(id){

        let task = this.tasks.filter(task =>task.id === id )[0]
        task.done = !task.done
        
      },
      deleteTask(id){
        this.tasks  = this.tasks.filter(task => task.id !== id ) //Cambiar el contenido del array y ellimina el que tiene id === task.id
         this.snackbarDelete = true
        
          
      },

      editButton(id){
        this. dialog = true  

        let edit= this.tasks.filter( task => task.id === id)[0].title
        
        this.editTaskTitle = edit
      },

      changeTaskTitle(id){
        this.dialog = false // Tengo que cambiar el title del model input con el title de la task

        for (let index = 0; index < this.tasks.length; index++) {

            if(this.tasks[index].id === id){
                this.tasks[index].title = this.editTaskTitle
                return
            }
          
        }
      },
      orderTask(){
          this.tasks.reverse()
          this.sortIcon = !this.sortIcon
      },
      deleteAllTask(){
        if(this.tasks.length === 0){
          this.snackbarDeleteAll = true
        }else{
          this.tasks = []
        }
        
       
    },

    deleteDoneTask(){


        let matchDone = 0
        for (let index = 0; index < this.tasks.length; index++) {
          
            if (this.tasks[index].done === true) {
              matchDone++
            }
        }

        if (matchDone === 0) {
            this.snackbarDeleteByDone = true
            this.textDeleteByDone = 'There is no task done!'
        }
        if(this.tasks.length === 0){
          this.snackbarDeleteByDone = true
          this.textDeleteByDone = 'There is no task!'
        }else{
             this.tasks = this.tasks.filter( task =>task.done !== true)
        }
        

      

        

       


    }

    
    },
  }
</script>


<style>
  .upButton{
    font-size: 10px;
  }
</style>
