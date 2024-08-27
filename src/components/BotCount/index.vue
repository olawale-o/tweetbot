<template>
  <div class="hello">
    <div>Bot Count {{ counts }}</div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
export default {
  name: "BotCount",
  props: {
    msg: String,
  },
  setup() {
    const counts = ref(0);
    const handleSSEMessage = (e) => {
      counts.value = JSON.parse(e.data).counts[0].value;
    };
    onMounted(() => {
      const eventSource = new EventSource("http://localhost:3000/stream");
      eventSource.addEventListener("message", handleSSEMessage);
    });
    return {
      counts,
    };
  },
};
</script>

<style scoped></style>
