<template >
  <v-app id="inspire" >
    
    <v-navigation-drawer  v-model="drawer" app>
      
          
      <v-list-item >
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            Vuetify Todo
          </v-list-item-title>
          <v-list-item-subtitle>
            Best todo ever!
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>
       

      <v-list dense nav>
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
          :to="item.to"
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      color="primary"
      dark
      prominent
      src="mountains.jpg"
      fade-img-on-scroll
      
      
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
        ></v-img>
      </template>

      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-app-bar-title class="text-h5">Vuetify Todo</v-app-bar-title>

      <v-spacer></v-spacer>
      
  
      


     
    </v-app-bar>

    <v-main>
      <Todo ></Todo>
    </v-main>
  </v-app>
</template>

<script>

import Todo from  "./views/Todo.vue"




  export default {
    components:{
      Todo,
     
    },
    data: () => ({ 
    
      drawer: null ,
      taskEmpty: '',
      taskFromChild: '',
      searchClosed: true,
      
      name:'App',
       items: [
          { title: 'Todo', icon: 'mdi-format-list-checks', to: '/' },
          { title: 'About', icon: 'mdi-help-box', to: '/about'},
        ],
  
      
      }),
      methods:{
      
      
     
      },
   
  }
</script>


<style lang="sass" >
  .v-input.expanding-search
    &.before, &.after
      max-width: 45px  
</style>