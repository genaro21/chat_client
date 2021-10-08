<template>
    <div class="chat">
        <div v-for="message in messages" :key="message._id">
           <p class="texto"> {{ message.text }}</p>
        </div> 

        <!-- <div>
            <br>
            <v-text-field v-model="text" placeholder="New message" class="mens"></v-text-field>
            <v-btn rounded color="success" @click="createMessage()">Send</v-btn>
        </div> -->

           
    </div>
</template>

<script>
export default {
    data() {
        return{
            messages: [],
            // userOneId: '',
            // userTwoId: '',
            // userOwnerId: userOneId,
            // text: '',
        };
    },
    async beforeMount() {
        const token = window.localStorage.getItem('token')
        if (!token) {
            this.$router.push('./login')
        }
        await this.getMessages(token)       
    },
    methods: {
        // async createMessage(token) {
        //     const body = {
        //     userOneId: window.localStorage.getItem('userId'),
        //     userTwoId: this.$route.params.id,
        //     userOwnerId:  window.localStorage.getItem('userId'),
        //     text: this.text, 
        //     }
            
        //     try {
        //         const res = await fetch('http://localhost:4500/message/create',{
        //             method: "post",
        //             headers: {
        //                  'Content-Type': 'application/json',
        //                   token: token,
        //             }
                
        //         })
        //     console.log(res)
            
        //     }catch (err) {
        //         alert(err)
        //     }
        // },
        
        async getMessages(token) {
            try{
                const userOneId = window.localStorage.getItem('userId')
                const userTwoId = this.$route.params.id

                console.log({ userOneId, userTwoId})
                const body = JSON.stringify({
                    userOneId,
                    userTwoId,
                })
               const res = await fetch('http://localhost:4500/message/chat', {
                   method: 'post',
                   headers: {
                       "Content-Type": "application/json",
                       token: token,
                   },
                   body
               }) 
               const data = await res.json()
               console.log({ data })
               const messages = data.messages

                for (let i = 0; i < messages.length; i++) {
                    this.messages.push(messages[i])
                }

            } catch (err) {
                console.log(err)
                alert(err)
            }
        }
    }
    
}
</script>
<style scoped>
.texto {
    color:black;
    font-size: 22px;
}
.mens {
    background-color: black;
    border-radius: 20px;
    font-size: 20px;
}
.boton {
    
}
</style>