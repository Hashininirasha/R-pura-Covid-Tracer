<template>
    
    <div class="container">
          
          <div :style="{backgroundImage:'url(bgg.jpg)'}">
              <div class="container">
    <h1 class="text-muted">Covid tracer app for</h1><br><h1 class="text-muted"> shops near by Ratnapura Bus stand</h1><br></div>
    <p class="text-justify"><span class="text-white bg-dark"> Ratnapura (Sinhala: රත්නපුර; Tamil: இரத்தினபுரி) ("City of Gems" in Sinhala and Tamil) is a major city in Sri Lanka. It is the capital city of Sabaragamuwa Province, as well as the Ratnapura District, and is a traditional centre for the Sri Lankan gem trade. It is located on the Kalu Ganga (Black River) in south-central Sri Lanka, some 101 km (63 mi) southeast of the country's capital, Colombo. Ratnapura is also spelled as Rathnapura.<br>The name 'Ratnapura' is a Sanskrit word meaning "city of gems", from the Sanskrit words pura (town) and ratna (gemstone).[1] Over 2000 years ago, when the first Buddhist monks arrived here from the north eastern provinces of India namely Bodh-Gaya, Varanasi and Pataliputra, they not only brought with them the Buddhist religion, but since their teachings were mainly in Sanskrit and Pali they also influenced the local language. While candy produced from the jaggery palm is traditionally known in this region as ratnapura, it is more likely that the candy was named for the locale rather than vice versa.[2]</span></p>
    <div v-if="loginStatus">
        <h1>Welcome {{user.displayName}}</h1>
        
        <select name="Places" v-model="selectedcheckin">
            <option
                v-for="Place in placelist"
                :value="Place.id"
                v-bind:key="Place.id"
                >{{Place.name}}</option>

        </select>
        <button v-on:click="signUserIn" >Cheack In </button>
    </div>
    <div v-else>
        <button v-on:click="doLogin" class="btn btn-danger">Sign In </button>
    </div>
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