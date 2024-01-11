<template>
  <div>
    <div class="container">
      <CustomTable :data="userData" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

let userData = ref([]);
const fetchData = async () => {
  const response = await fetch("https://randomuser.me/api/?results=50");
  const data = await response.json();

  userData.value = data.results;
  userData.value.sort((a, b) => {
    let fa = a.name.first.toLowerCase(),
      fb = b.name.first.toLowerCase();

    if (fa < fb) {
      return -1;
    }
    if (fa > fb) {
      return 1;
    }
    return 0;
  });
};

onMounted(() => {
  fetchData();
});
</script>

<script>
// Import the CustomTable component
import CustomTable from "@/components/customTable.vue";

export default {
  components: {
    CustomTable,
  },
};
</script>
<style scoped>
html,
body {
  height: 100%;
}

body {
  margin: 0;
  background: #e0e5ec;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 100;
}
</style>
