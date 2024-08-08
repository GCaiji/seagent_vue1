<template>
  <div class="sidebar">
    <div class="newchat">
      <button @click="newChat">New Chat</button>
    </div>
    <div class="chatlist">
      <div class="chat-item" v-for="(chat, index) in chats.slice().reverse()" :key="index" @click="selectChat(chat.id)"
        :class="{ active: chat.id === selectedChatId }">
        <button>
          <div>
            <div v-if="!chat.editing" class="editicon" @click="toggleEdit(chat)"></div>
            <div v-else class="editicon-save" @click="saveEdit(chat)"></div>
          </div>
          <div class="itemtitle">
            <template v-if="chat.editing">
              <input type="text" v-model="editedTitle" @keyup.enter="saveEdit(chat)" @blur="saveEdit(chat)">
            </template>
            <template v-else>
              {{ chat.title }}
            </template>
          </div>
          <div class="deleteicon" @click.stop="deleteChat(chat.id)"></div>
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
        title: `New Chat`, // 给对话选项一个标题
        id: newChatId,

      });
      this.selectChat(newChatId);
    },
    selectChat(chatId) {
      this.selectedChatId = chatId;
    },
    deleteChat(chatId) {
      // 根据 chatId 删除对应的聊天选项
      const index = this.chats.findIndex(chat => chat.id === chatId);
      if (index !== -1) {
        // 如果只剩一个聊天选项，弹出提示框并返回
        if (this.chats.length === 1) {
          alert("无法删除所有聊天框");
          return;
        }

        this.chats.splice(index, 1);

        if (this.selectedChatId === chatId) {
          // 如果删除的是当前选中的聊天，将选中聊天切换为上一个聊天（如果有的话）
          if (index > 0) {
            this.selectChat(this.chats[index - 1].id);
          } else {
            this.selectChat(this.chats[0].id);
          }
        }
      }
    },
    toggleEdit(chat) {
      // 切换编辑状态
      chat.editing = !chat.editing;
      // 如果进入编辑状态，则设置编辑框的默认值为原标题
      if (chat.editing) {
        this.editedTitle = chat.title;
      }
    },
    saveEdit(chat) {
      // 保存编辑后的内容
      chat.title = this.editedTitle;
      chat.editing = false; // 退出编辑状态
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
  min-width: 220px;
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
  min-width: 220px;
}

.chat-item button:hover {
  background-color: #f1f1f1
}

.chat-item button .itemtitle {
  position: absolute;
  left: 10%;
  top: 50%;
  width: 40%;
  transform: translateY(-50%);
  height: 20px;
  font-size: 18px;
}

.itemtitle input {
  position: absolute;
  left: 0;
  width: 100%;
  min-width: 88px;
  height: 18px;
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
  width: 20px;
  height: 20px;
  background-image: url(../images/icon/delete.png);
  background-size: contain;
}

.deleteicon:hover {
  background-image: url(../images/icon/delete_hover.png);
}

.editicon {
  position: absolute;
  right: 28%;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  background-image: url(../images/icon/edit.png);
  background-size: contain;
}

.editicon:hover {
  background-image: url(../images/icon/edit_hover.png);
}

.editicon-save {
  position: absolute;
  right: 28%;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  background-image: url(../images/icon/save.png);
  background-size: contain;
}

.editicon-save:hover {
  background-image: url(../images/icon/save_hover.png);
}
</style>