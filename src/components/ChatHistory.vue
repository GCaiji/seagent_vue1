<template>
  <div class="sidebar">
    <div class="newchat">
      <button @click="newChat">New Chat</button>
    </div>
    <div class="chatlist">
      <div class="chat-item" v-for="(chat, index) in chats.slice().reverse()" :key="index" @click="selectChat(chat.id)"
        :class="{ active: chat.id === selectedChatId }">
        <button><span>{{ chat.title }}</span>
          <div class="deleteicon"></div>
        </button>
      </div>
    </div>
  </div>
  <div class="chat-container">
    <keep-alive>
      <Chat v-if="selectedChatId !== null" :key="selectedChatId" :chat-id="selectedChatId" />
    </keep-alive>
  </div>
</template>

<script>
import Chat from './Chat.vue';
export default {
  components: {
    Chat
  },
  data() {
    return {
      chats: [],
      nextChatId: 1,
      selectedChatId: null
    };
  },
  created() {
    // 创建默认的初始对话框和选项
    this.newChat();
  },
  methods: {
    newChat() {
      const newChatId = this.nextChatId++;
      const newChatTitle = `Chat ${newChatId}`;
      // 在数组中添加一个新的对话选项对象
      this.chats.push({
        title: `New Chat_${this.chats.length + 1}`, // 给对话选项一个标题
        id: newChatId,

      });
      this.selectChat(newChatId);
    },
    selectChat(chatId) {
      this.selectedChatId = chatId;
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
  position: relative;
  margin-bottom: 10px;
  width: 90%;
}

.chat-item button {
  position: relative;
  display: flex;
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

.chat-item button span {
  position: absolute;
  left: 25%;
  top: 50%;
  transform: translateY(-50%);
  height: 20px;
  font-size: 16px;
}

.chat-item.active button {
  border-color: rgb(75 158 95);
  color: rgb(75 158 95);
}

.chat-item.active button:hover {
  background-color: white;
}

.deleteicon {
  position: absolute;
  right: 16%;
  top: 50%;
  transform: translateY(-50%);
  width: 18px;
  height: 18px;
  background-image: url(../images/delete.png);
  background-size: contain;
}

.deleteicon:hover {
  background-image: url(../images/delete_hover.png);
}
</style>