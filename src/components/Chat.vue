<script setup>
    // import ref, reactive
    import { ref, reactive, onMounted } from 'vue'  //reactive is voor complexere objecten

    let messages = ref(['yo', 'yoyo']);
    let newMessage = ref('');
    let socketServer = (null);

    onMounted(() => {
        //socketServer = new WebSocket('ws://localhost:3000/primus'); // ws = geen secure, zoals http
        socket = new WebSocket('wss://backendsocks.onrender.com/primus')

        // listen for data from server
        socketServer.onmessage = (event) => {
            let data = JSON.parse(event.data);
            console.log(data);
            if (data.action == 'newMessage') {
                messages.value.push(data.message);
            }
        }
    });

    const sendMessage = () => {
        //messages.value.push(newMessage.value);
    let m = {
    "action": "newMessage",
    "message": newMessage.value
    };
    socketServer.send(JSON.stringify(m));
    
} 
    

</script>

<template>
  <div>
    <h1>Chat </h1>
    <ul>
        <li v-for="m in messages">{{ m }}</li>
    </ul>
    <div>
        <input type="text" v-model="newMessage" />
        <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style scoped>
  
</style>
