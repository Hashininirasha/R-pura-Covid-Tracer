<template>
    <div>
    <h1>Covid tracer app for shops near by Ratnapura Bus stand</h1>
    <div v-if="loginStatus">
        <button>Cheack In </button>
    </div>
    <div v-else>
        <button v-on:click="doLogin">Sign In </button>
    </div>
    </div>
</template>

<script>
import firebase from 'firebase/app'
import 'firebase/auth'

export default {
    name: 'Tracer',
        data() {
            return{
                loginStatus: false
            }
        },
        methods : {
            doLogin() {
                let provider = new firebase.auth.GoogleAuthProvider();
                firebase.auth().signInWithPopup(provider)
                .then((response)=>{
                    console.log(response);
                })
                .catch((error)=>{
                    console.error(error);
                })
            }
        }
    
}
</script>
<style scoped>

</style>