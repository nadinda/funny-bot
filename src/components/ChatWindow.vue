<template>
  <div class="chat-container">
    <div class="chat-messages">
      <div class="message-bot">
        <div class="message-text">Welcome! I'm Funny Bot</div>
        <div class="message-sender">bot</div>
      </div>
      <div class="message-user">
        <div class="message-text">Hello, I'm user</div>
        <div class="message-sender">user</div>
      </div>
    </div>
    <div class="chat-input">
      <form>
        <input type="text" placeholder="Type your message..." />
      </form>
    </div>
  </div>
</template>

<script setup lang="ts"></script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 90vh;
  width: 100%;
  background-color: rgb(255, 255, 205);
}

.chat-messages {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}

.chat-messages .message-bot {
  display: flex;
  flex-direction: row-reverse;
  justify-content: flex-end;
  margin: 10px;
}

.chat-messages .message-user {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  margin: 10px;
}

.chat-messages .message-text {
  padding: 10px;
  border-radius: 10px;
  background-color: #ffffff;
  font-size: 16px;
  word-wrap: break-word;
}

.chat-messages .message-sender {
  font-size: 12px;
  margin-top: 5px;
  margin-left: 10px;
  margin-right: 10px;
}

.message-bot {
  display: flex;
  justify-content: flex-start;
  margin-bottom: 10px;
}

.message-user {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 10px;
}

.chat-input input[type="text"] {
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  outline: none;
}
</style>
