<template>
  <div id="app">
    <Container>
      <ChatWindow @send-message="sendMessage">
        <ChatMessage v-for="message in data" :key = "message.id" :username="message.author" :time="message.datetime">{{message.text}}</ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import axios from 'axios'
import Container from './components/Container.vue'
import ChatWindow from './components/ChatWindow.vue'
import ChatMessage from './components/ChatMessage.vue'

export default {
  data() {
    return {
      data: []
    }
  },
  components: {
   Container,
   ChatWindow,
   ChatMessage
  },
  methods: {
    sendMessage(obj){
        axios.post('http://188.225.47.187/api/chat/sendmessage.php', {
        author: obj.nickname,
        text: obj.message})
        .then((response)=>{
            this.getMessages();
            console.log(response);
          })
        
    },
    getMessages(){
      axios.get('http://188.225.47.187/api/chat/getmessages.php')
      .then((response) =>{
        console.log('response', response)
        this.data = response
      })
    }
  },
  mounted(){
    this.getMessages();
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>
