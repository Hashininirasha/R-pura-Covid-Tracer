<template>
    <div>
    <h1>Covid tracer app for shops near by Ratnapura Bus stand</h1>
    <div v-if="loginStatus">
        <h1>Welcome {{user.displayName}}</h1>
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
import 'firebase/firestore'

export default {
    name: 'Tracer',
        data() {
            return{
                loginStatus: false,
                user: null
            }
        },
        methods : {
            doLogin() {
                let provider = new firebase.auth.GoogleAuthProvider();
                firebase.auth().signInWithPopup(provider)
                .then((response)=>{
                    console.log(response);
                    this.loginStatus = true;
                    this.user = response.user
                    this.db=firebase.firestore();
                    this.saveUser(this.user);
                    
                })
                .catch((error)=>{
                    console.error(error);
                })
            },
            saveUser(user){
                    this.db.collection("users").doc(user.uid).set({
                        id : user.uid,
                        displayName : user.displayName
                    })
                    .then((documentReference)=>{
                        console.log(documentReference)
                        console.loginStatus("user added");
                    })

            }
        }
    
}
</script>
<style scoped>

</style>