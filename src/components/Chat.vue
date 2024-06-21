<template>
  <div class="chat-container">
    <div class="chat-box" v-for="(message, index) in messages" :key="index">
      <div :class="['message', message.sender]">
        <span>{{ message.sender }}:</span> {{ message.text }}
      </div>
    </div>
    <div class="input-box">
      <input v-model="userInput" @keyup.enter="sendMessage" placeholder="输入消息..."/>
      <button @click="sendMessage">发送</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      messages: [],
      userInput: '',
    };
  },
  methods: {
    async sendMessage() {
      if (this.userInput.trim() !== '') {
        this.messages.push({sender: '用户', text: this.userInput});
        await this.getResponse(this.userInput); // 等待获取回复后再清空输入框
        this.userInput = '';
      }
    },

    async getResponse(input) {
      try {
        const requestBody = {message: input};
        console.log('Sending request with body:', requestBody);
        const response = await axios.post('http://127.0.0.1:8000/api/chat_with_model', requestBody, {
          headers: {
            'Content-Type': 'application/json' // 设置请求头部
          }
        });

        this.messages.push({sender: '助手', text: response.data.result});
      } catch (error) {
        console.error('HTTP error', error.response ? error.response.status : error);
      }
    },
  },
};
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  background-color: #f9f9f9;
}

.chat-box {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
}

.message {
  padding: 10px;
  border-radius: 5px;
  margin-bottom: 5px;
}

.message.用户 {
  align-self: flex-end;
  background-color: #daf8da;
}

.message.助手 {
  align-self: flex-start;
  background-color: #f1f0f0;
}

.input-box {
  display: flex;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px 0 0 5px;
}

button {
  padding: 10px;
  border: none;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  border-radius: 0 5px 5px 0;
}

button:hover {
  background-color: #0056b3;
}
</style>
