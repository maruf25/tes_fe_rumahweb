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
    user.value = data;
  } catch (error) {
    console.log(error);
  }
};

watch(
  () => route.params.id,
  () => {
    getUser();
  },
  { immediate: true }
);
</script>

<template>
  <div class="p-4">
    <div class="text-black text-xl p-4 font-bold">
      <a href="/users"> <div class="text-blue-500">back</div></a>
      <!-- <RouterLink :to="{ path: '/users' }" replace>
        <div class="text-blue-500">back</div>
      </RouterLink> -->
      <h1>User Profile</h1>
    </div>
    <div class="bg-red-200 rounded-lg shadow-md p-4 gap-4">
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
