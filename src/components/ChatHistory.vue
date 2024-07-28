<template>
  <div class="sidebar">
    <div class="newchat">
      <button @click="newChat">New Chat</button>
    </div>
    <div class="chatlist">
      <div v-for="(chat, index) in chats" :key="index" class="chat-item">
        <button>{{ chat.title }}</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      chats: [
        { title: 'New Chat_1' } // 初始的对话选项
      ]
    };
  },
  methods: {
    newChat() {
      // 在数组中添加一个新的对话选项对象
      this.chats.push({
        title: `New Chat_${this.chats.length + 1}`  // 给对话选项一个标题
      });
    }
  }
};
</script>

<style scoped>
.sidebar {
  position: absolute;
  bottom: 0px;
  left: 0px;
  border: 1px solid #ccc;
  border-left: 0px;
  border-bottom: 0px;
  border-right: 0px;
  width: 18%;
  height: 86%;
}

.newchat {
  display: flex;
  width: 100%;
  height: 66px;
  justify-content: center;
  align-items: center;
}

.newchat button {
  width: 90%;
  height: 70%;
  border: 1px dashed #ccc;
  background-color: white;
  border-radius: 7px;
  cursor: pointer;
}

.newchat button:hover {
  background-color: #f1f1f1
}

.chatlist {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: calc(100% - 66px);
  overflow-y: auto;
}

.chatlist::-webkit-scrollbar {
  width: 6px;
}

.chatlist::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.chatlist::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 10px;
}

.chatlist::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}

.chat-item {
  margin-bottom: 10px;
  width: 90%;
}

.chat-item button {
  width: 100%;
  height: 50px;
  cursor: pointer;
  background-color: white;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.chat-item button:hover {
  background-color: #f1f1f1
}
</style>