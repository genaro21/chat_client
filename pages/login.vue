<template>
    <div class="login text-center">
       <p class="color"> <v-text-field v-model="email" placeholder = "Email"> </v-text-field></p>
        <v-text-field v-model="password" placeholder = "Password" type="password"> </v-text-field>
        <v-btn elevation="24" x-large @click="onLogin">Submit</v-btn>

    </div>
</template>

<script>
export default{
    data() {
        return {
            email: '',
            password: '',
        }
    },
    methods: {
        async onLogin() {
            const body = {
                email: this.email,
                password: this.password,
            }

            try {
                const res = await fetch('http://localhost:4500/user/login', {
                method: 'post',
                headers: {
                    'Content-Type' : 'application/json',
                },
                body: JSON.stringify(body),
            })

            const data = await res.json()
            if(data.error) {
                alert(data.error)
                return
            }
            console.log(data)
            window.localStorage.setItem('token', data.token)
            window.localStorage.setItem('userId', data.userId)
            window.localStorage.setItem('admin', data.admin)
            this.$router.push('./users')
          } catch (err) {
                alert(err)
            }
          
        },
    },
} 

</script>


<style scoped>

</style>