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
import { ref, onMounted } from "vue";

interface Message {
  content: string;
  sender: "user" | "bot";
}

interface Joke {
  [key: string]: string[];
}

enum ChatState {
  SELECT_CATEGORY,
  ASK_NEW_JOKE,
  END,
}

const messages = ref<Message[]>([]);
const userInput = ref("");
const categories = ["pun", "programming", "money"];
const chatStatus = ref(ChatState.SELECT_CATEGORY);

const jokes: Joke = {
  pun: [
    "What kind of TV does a skeleton watch? A skelevision.",
    "What do you get if you cross a teddy bear with a pig? A teddy boar.",
    "What do you call a cow with a twitch? Beef jerky.",
  ],
  programming: [
    "Why do programmers prefer dark mode? Because light attracts bugs.",
    "Why do Java Programmers have to wear glasses? Because they can't see C#",
    "What happens when developers ask a silly question? They get a silly ANSI.",
  ],
  money: [
    "Where will you always find money? In a dictionary",
    "Where do frogs put his money? In the river bank.",
    "What did one penny say to the other penny? Us being together just makes cents.",
  ],
};

const pushBotMessage = (content: string) => {
  messages.value.push({
    content,
    sender: "bot",
  });
};

const pushUserMessage = (content: string) => {
  messages.value.push({
    content,
    sender: "user",
  });
};

const initBotMessage = () => {
  pushBotMessage(
    "Welcome! Do you want to hear a joke? Choose a category by typing: 'pun', 'programming', 'money'"
  );
};

const requestJokeCategory = () => {
  chatStatus.value = ChatState.SELECT_CATEGORY;
  pushBotMessage(
    "Please choose a category by typing: 'pun', 'programming', 'money'"
  );
};

const handleJokeCategoryRequest = (selectedCategory?: string) => {
  if (selectedCategory) {
    const jokeIndex = Math.floor(
      Math.random() * jokes[selectedCategory].length
    );
    pushBotMessage(jokes[selectedCategory][jokeIndex]);
    requestNewJoke();
  } else {
    pushBotMessage(
      "I didn't understand. Please choose a category by typing: 'pun', 'programming', 'money'"
    );
  }
};

const requestNewJoke = () => {
  chatStatus.value = ChatState.ASK_NEW_JOKE;
  pushBotMessage("Do you want to hear another joke? Type 'yes' or 'no'");
};

const handleNewJokeRequest = (input: string) => {
  if (input.includes("yes")) {
    requestJokeCategory();
  } else if (input.includes("no")) {
    requestEndChat();
  } else {
    pushBotMessage("I didn't understand. Please type 'yes' or 'no'");
  }
};

const requestEndChat = () => {
  chatStatus.value = ChatState.END;
  pushBotMessage("Okay, see you later!");
};

const sendMessage = () => {
  if (userInput.value.trim() !== "") {
    pushUserMessage(userInput.value);
    const input = userInput.value.toLowerCase();
    const category = getCategoryFromInput(input);

    if (chatStatus.value === ChatState.SELECT_CATEGORY) {
      handleJokeCategoryRequest(category);
    } else if (chatStatus.value === ChatState.ASK_NEW_JOKE) {
      handleNewJokeRequest(input);
    }
    userInput.value = "";
  }
};

const getCategoryFromInput = (input: string) => {
  return categories.find((category) => input.includes(category));
};

onMounted(() => {
  initBotMessage();
});
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
