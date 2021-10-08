<template>
    <div class="signUp text-center">
        <v-text-field style="color: black;" v-model="email" placeholder = "Email"> </v-text-field>
        <v-text-field v-model="password1" placeholder = "Password" type="password"> </v-text-field>
        <v-text-field v-model="password2" placeholder = "Repeat Password" type="password"> </v-text-field>
        <v-btn  elevation="24" x-large @click="signUp">Submit</v-btn>

    </div>
</template>

<script>
export default {
    data() {
        return {
            email: '',
            password1: '',
            password2: '',
        }

    },
    methods: {
        async signUp () {            
            const body = {
                email: this.email,
                password1: this.password1,
                password2: this.password2,
            }
             
            try {
                const res = await fetch('http://localhost:4500/user/create',{
                method: "post",
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(body),
            })

            const data = await res.json()
           
            if (data.error) {
                alert(data.error)
            }console.log({ data })
            
           this.$router.push('./signUp')


            } catch (err) {
                alert(err)
            }
        this.$router.push('./login')
        },
    },
}
    
</script>

<style scoped>
.color {
    color: black
}
</style>
