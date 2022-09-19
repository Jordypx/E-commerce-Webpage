<template>
  <div>
    <HeaderComponent />
    <Collection />
  </div>
 </template>
 
 <script>
 import HeaderComponent from '../components/HeaderComponent.vue'
 import Collection from '../components/Collection.vue';
 
 
 export default {
   name: 'App',
   components: {
     HeaderComponent,
     Collection
 }
 }
 </script>
