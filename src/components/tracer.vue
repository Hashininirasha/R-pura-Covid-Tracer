<template>
    <div>
    <h1>Covid tracer app for shops near by Ratnapura Bus stand</h1>
    <div v-if="loginStatus">
        <button>Cheack In </button>
    </div>
    <div v-else>
        <button>Sign In </button>
    </div>
    </div>
</template>
<script>
export default {
    name: 'Tracer',
        data(){
            return{
                loginStatus: false
            }
        }
    
}
</script>
<style scoped>

</style>