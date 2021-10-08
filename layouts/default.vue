<template>
  <v-app class="body">
    <v-container class="mt-12">
        <v-row>
            <v-col cols="3">
            </v-col>  
            <v-col class="intro" cols="6">
                 <Nuxt />
            </v-col> 
            <v-col cols="3">
            </v-col>   
        </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {}
</script>

<style scoped>
.intro {
    background-color: rgb(248, 205, 86);
    margin-top: 100px;
    border-radius: 40px;
    box-shadow: 10px 10px 20px rgb(140,140,140);
    
}
.body {
    background-color: wheat;
    border-radius: 50%;
}
</style>