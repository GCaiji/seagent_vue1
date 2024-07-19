<template>
  <div class="chat-container">
    <div class="chat-box" ref="chatBox">
      <div class="message" v-for="(message, index) in messages" :key="index">
        <div :class="['message', message.sender]">
          <span>{{ message.sender }}:</span> <span v-html="message.text"></span> <!-- 使用 v-html 渲染 HTML -->
        </div>
      </div>
    </div>
    <div class="input-box">
      <input v-model="userInput" @keyup.enter="sendMessage" type="text" placeholder="输入消息..."/>
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
        this.messages.push({sender: '用户', text: this.userInput});
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
            'Content-Type': 'application/json'
          }
        });

        const html = marked.parse(response.data.message); // 使用 marked 将 Markdown 转换为 HTML
        this.messages.push({sender: '助手', text: html});
      } catch (error) {
        console.error('HTTP error', error.response ? error.response.status : error);
      }
    },

    scrollChatToBottom() {
      this.$nextTick(() => {
        const chatBox = this.$refs.chatBox;
        chatBox.scrollTop = chatBox.scrollHeight;
      });
    },
  },
};
</script>

<style scoped>
.chat-container {
  position: absolute;
  right: 0;
  top: 0;
  width: 1150px;
  height: 100%;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  background-color: #f9f9f9;
  overflow: hidden;
}

.chat-box {
  height: calc(100% - 60px);
  overflow-y: auto;
  padding: 10px;
  border-radius: 10px;
  background-color: #fff;
  border: 1px solid #ccc;
}

.message {
  margin-bottom: 10px;
  text-align: right;
}

.message-content {
  padding: 8px 12px;
  border-radius: 5px;
  display: inline-block;
  max-width: 70%;
}

.message-用户 {
  align-self: flex-end;
  word-wrap: break-word;
  background-color: #daf8da;
}

.message-助手 {
  align-self: flex-start;
  word-wrap: break-word;
  background-color: #f1f0f0;
}

.input-box {
  display: flex;
  margin-top: 10px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px 0 0 5px;
}

button {
  padding: 10px 20px;
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
