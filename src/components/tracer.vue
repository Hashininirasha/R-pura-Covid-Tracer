<template>
    <div>
    <h1>Covid tracer app for shops near by Ratnapura Bus stand</h1>
    <div v-if="loginStatus">
        <h1>Welcome {{user.displayName}}</h1>
        <select name="Places" v-model="selectedcheckin">
            <option
                v-for="Place in placelist"
                :value="Place.id"
                v-bind:key="Place.id"
                >{{Place.name}}</option>

        </select>
        <button v-on:click="signUserIn">Cheack In </button>
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
                user: null,
                placelist: []
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
                    this.popplaces();
                    
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
                    });

            },
            popplaces(){
                this.db.collection('Places').get()
                .then((result)=>{
                    result.forEach((place)=>{
                        console.log(place.data());
                        this.placelist.push({
                            'name':place.data().name,
                            'id':place.id,

                        });
                    })
                })
                .catch((error)=>{
                    console.log(error);
                })
            },
            signUserIn(){
                this.db.collection("Cheack In").add({
                    placeID : this.selectedcheckin,
                    timeIn : new Date(),
                    checkOut : null,
                    userID : this.user.uid
                })
                .then((result)=> {
                    console.log(result);
                    console.log("User Checked in");
                })
            }
        }
    
}
</script>
<style scoped>

</style>