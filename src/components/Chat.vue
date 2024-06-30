<template>
  <div class="chat-container">
    <div class="chat-box" v-for="(message, index) in messages" :key="index">
      <div :class="['message', message.sender]">
        <span>{{ message.sender }}:</span> <span v-html="message.text"></span> <!-- 使用 v-html 渲染 HTML -->
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
import * as marked from 'marked';

export default {
  data() {
    return {
      messages: [],
      userInput: '',
      userId: 1, // 假设用户 ID 是 1，你可以从实际的用户数据中获取
      role: 'user', // 假设用户角色是 'user'，你可以根据实际情况设置
    };
  },
  methods: {
    async sendMessage() {
      if (this.userInput.trim() !== '') {
        this.messages.push({ sender: '用户', text: this.userInput });
        await this.getResponse(this.userInput); // 等待获取回复后再清空输入框
        this.userInput = '';
      }
    },

    async getResponse(input) {
      try {
        const requestBody = {
          user_id: this.userId,
          message: input,
          role: this.role
        };
        console.log('Sending request with body:', requestBody);
        const response = await axios.post('http://127.0.0.1:8000/api/chat_with_model', requestBody, {
          headers: {
            'Content-Type': 'application/json' // 设置请求头部
          }
        });

        const html = marked.parse(response.data.message); // 使用 marked 将 Markdown 转换为 HTML
        this.messages.push({ sender: '助手', text: html });
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
