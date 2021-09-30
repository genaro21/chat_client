<template>
    <div class="chat">
        <div v-for="message in messages" :key="message._id">{{ message.text }}</div> 
            
           
    </div>
</template>

<script>
export default {
    data() {
        return{
            messages: []
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