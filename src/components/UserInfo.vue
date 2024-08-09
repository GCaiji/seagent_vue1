<template>
  <div class="chat-container">
    <div class="user-info-container">
      <div class="user-info" v-if="user">
        <img :src="user.avatar" alt="User Avatar" class="user-avatar">
        <div class="user-details">
          <span>{{ user.name }}</span>
          <small>{{ user.role }}</small>
        </div>
      </div>
    </div>

    <!-- 清空聊天记录按钮 -->
    <!-- <button @click="clearMessages" class="clear-button">清空聊天记录</button> -->
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
      user: {
        // 假设的用户信息
        name: '张三',
        role: '用户',
        avatar: "https://raw.githubusercontent.com/nullskymc/blogs/master/images/avatar.jpg?token=GHSAT0AAAAAACPOUJUBUC7DCAN6ROK4T3LOZVUS7HA", // 这里可以是默认头像的路径
      },
      userId: 1, // 保持这个字段，即使它不被使用
      role: 'user', // 保持这个字段，即使它不被使用
    };
  },

  methods: {

  },
};
</script>

<style scoped>
.user-info-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 18%;
  height: 14%;
  z-index: 100;
  background-color: #fff;
  border: 0px solid #ccc;
  border-radius: 5px;
  padding: 5px 10px;
}

.user-info {
  display: flex;
  align-items: center;
}
.user-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}
</style>
