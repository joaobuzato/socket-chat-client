<template>
  <div>
    <div v-for="(msg, index) in messages" :key="index">
      <span :style="{ color: msg.color }"
        >{{ msg.username }}: {{ msg.message }}</span
      >
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";
import { io } from "socket.io-client";

const socket = io("http://localhost:3000", { path: "/chat" });

export default {
  name: "MessageList",
  props: {
    messages: {
      type: Array,
      required: true,
    },
  },
  setup(props) {
    console.log("MessageList setup", props.messages);
    const messages = ref(props.messages);

    const addMessage = (msg) => {
      messages.value.push(msg);
    };

    onMounted(() => {
      socket.on("chat message", addMessage);
    });

    onUnmounted(() => {
      socket.off("chat message", addMessage);
    });

    return { messages };
  },
};
</script>
