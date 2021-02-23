<template>
<Tracer/>
  
</template>

<script>
import firebase from 'firebase/app'
import Tracer from './components/tracer.vue'
var firebaseConfig = {
    apiKey: "AIzaSyAJkHRkogtyZ9eB1eDB4ia_ztrG1_3JnL4",
    authDomain: "r-pura-covid-tracer.firebaseapp.com",
    projectId: "r-pura-covid-tracer",
    storageBucket: "r-pura-covid-tracer.appspot.com",
    messagingSenderId: "402661802423",
    appId: "1:402661802423:web:3e55049d6fa31067646832",
    measurementId: "G-V2TEC37MRX"
  };
  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);
  export default{
    name: 'App',
      components:{
        Tracer
      }
    
  }

</script>


<style>

</style>
