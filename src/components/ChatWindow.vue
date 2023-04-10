<template>
  <div class="chat-container">
    <div class="chat-messages">
      <div
        v-for="(message, index) in messages"
        :key="index"
        :class="{
          'message-bot': message.sender === 'bot',
          'message-user': message.sender === 'user',
        }"
      >
        <div class="message-text">{{ message.content }}</div>
        <div class="message-sender">{{ message.sender }}</div>
      </div>
    </div>
    <div class="chat-input">
      <form @submit.prevent="sendMessage">
        <input
          v-model="userInput"
          type="text"
          placeholder="Type your message..."
        />
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

interface Message {
  content: string;
  sender: "user" | "bot";
}

const messages = ref<Message[]>([]);
const userInput = ref("");

const sendMessage = () => {
  if (userInput.value.trim() !== "") {
    messages.value.push({
      content: userInput.value,
      sender: "user",
    });

    messages.value.push({
      content: "hi",
      sender: "bot",
    });

    userInput.value = "";
  }
};
</script>

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
