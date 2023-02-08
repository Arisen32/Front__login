<template>
    <v-app>
      <v-navigation-drawer 
      
      absolute
      permanent
      app
      >

       <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
           Arisen32
          </v-list-item-title>
          <v-list-item-subtitle>
               sistema
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      </v-navigation-drawer>
  
      <v-app-bar app>
        
  
        <v-toolbar-title>CRUD con mongo DB y nuxt 🐱‍👤</v-toolbar-title>
      </v-app-bar>
  
      <v-main>
        <Nuxt />
      </v-main>
    </v-app>
  </template>
  
  <script>
    export default {
      data: () => ({ drawer: null }),
    }
  </script>