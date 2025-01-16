<script setup>
import { ref, watch } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const user = ref({});
const userId = route.params.id;

const getUser = async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users/" + userId);
    if (!response.ok) {
      const error = {
        status: response.status,
        message: response.statusText,
      };
      throw error;
    }

    const data = await response.json();
    console.log(data);
    console.log(data.address.street);

    user.value = data;
  } catch (error) {
    console.log(error);
  }
};

watch(
  () => [route.params.id],
  () => {
    getUser();
  },
  { immediate: true }
);
</script>

<template>
  <div class="w-full p-4 text-xl font-bold text-black i">
    <RouterLink :to="{ path: '/users' }">
      <div class="inline-block p-2 text-white bg-blue-500 rounded-lg">back</div>
    </RouterLink>
    <h1>Detail User</h1>
  </div>

  <div v-if="Object.keys(user).length === 0" class="flex items-center justify-center h-96">
    <h1 class="text-2xl font-bold">Users is empty</h1>
  </div>

  <div class="p-4">
    <div v-if="Object.keys(user).length > 0" class="p-4 bg-red-200 rounded-lg shadow-md">
      <h1>Name : {{ user.name }}</h1>
      <h1>Email :{{ user.email }}</h1>
      <h1>
        Address : {{ user.address.street }}, {{ user.address.suite }}, {{ user.address.city }},
        {{ user.address.zipcode }}
      </h1>
      <h1>Phone : {{ user.phone }}</h1>
      <h1>Websie : {{ user.website }}</h1>
      <h1>Company : {{ user.company.name }}</h1>
    </div>
  </div>
</template>
