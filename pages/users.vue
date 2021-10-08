<template>
    <div class="users">
        
                <div v-if="isAdmin">
            <div><h2>Usuarios</h2></div>
            <div v-for="user in users" :key="user._id">
                
                <p class="nombre">{{ user.email }}</p>
                <v-btn class="mb-8" @click="redirect(user._id)">Chat</v-btn>
                <v-btn color="error" class="mb-8" @click="remove(user._id)">Delete</v-btn>
            </div>
        </div>    
        <div v-else>
            <div><h2>Usuarios</h2></div>
            <div v-for="user in users" :key="user._id">
                <p class="nombre">{{ user.email }}</p>
                <v-btn class="mb-8" @click="redirect(user._id)">Chat</v-btn>
            </div>
        </div> 
        <div>
            <v-btn @click="logOut()">Logout</v-btn>
        </div>

    
      
    </div>
</template>

<script>
export default {
    data() {
        return {
            users: [],
            isAdmin: "",
        }
    },

    async beforeMount() {
        const token = window.localStorage.getItem('token')
        if (!token) {
            this.$router.push('./login')
        }
        await this.getAllUsers(token)
        this.isAdmin = window.localStorage.getItem('admin') === 'true'
    },
    methods: {
        logOut() {
            console.log('Borrando')
            window.localStorage.clear()
            this.$router.push(`/login`) 
             

            
        },

        async remove(id) {
           await fetch('http://localhost:4500/user/remove/'+id, {
               method: "delete",
           })
        },

        redirect(id) {
         this.$router.push(`/chat/${id}`)   
        },

        async getAllUsers (token) {
            
            try {
                const res = await fetch('http://localhost:4500/user/all', { 
                    method: 'get',
                    headers: {
                        token: token,
                    },
                })
                const data = await res.json()
                if(data.error) {
                    alert(data.error)
                    return this.$router.push('/login')
                }  
                const users = data.users

                for (let i = 0; i < users.length; i++) {
                    this.users.push(users[i])
                }
              } catch (err) {
                alert(err)
              }
       
        },
    },
}
</script>

<style scoped>
.nombre{
    color:black;
    font-size: 22px;
}
h2 {
    text-align: center;
    color: black
}
</style>
