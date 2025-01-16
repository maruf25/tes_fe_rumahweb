<script setup>
import { ref, watch } from "vue";
import { useRoute } from "vue-router";
const users = ref([]);
const search = ref("");
const route = useRoute();

const getUsers = async () => {
  let url = "https://jsonplaceholder.typicode.com/users";
  if (search.value !== "") {
    url = "https://jsonplaceholder.typicode.com/users?email=" + search.value;
  }
  try {
    const response = await fetch(url);
    if (!response.ok) {
      const error = {
        status: response.status,
        message: response.statusText,
      };
      throw error;
    }

    const data = await response.json();
    users.value = data;
  } catch (error) {
    console.log(error);
  }
};

watch(
  () => [search.value, route.path],
  () => {
    getUsers();
  },
  { immediate: true }
);

const inputSearch = (e) => {
  const { value } = e.target;

  search.value = value;
};
</script>

<template>
  <div class="flex w-full text-black text-xl font-bold p-4 justify-between items-center">
    <h1>Users</h1>
    <div>
      <input
        class="p-2 border rounded-lg"
        id="search"
        name="search"
        type="text"
        placeholder="search by email"
        v-model="search"
        @input="inputSearch"
      />
      <!-- <button class="bg-blue-500">Search</button> -->
    </div>
  </div>

  <div v-if="users.length <= 0" class="flex items-center justify-center h-96">
    <h1 class="text-2xl font-bold">Users is empty</h1>
  </div>

  <div class="gap-4">
    <div v-for="user in users" :key="user">
      <RouterLink :to="{ name: 'DetailsUser', params: { id: user.id } }" replace>
        <!-- <RouterLink :to="{ path:"/users/" + user.id}"> -->
        <div class="bg-red-200 p-4 rounded-lg m-4 shadow-lg">
          <h1>Name : {{ user.name }}</h1>
          <h1>Email : {{ user.email }}</h1>
          <h1>
            Address : {{ user.address.street }}, {{ user.address.suite }}, {{ user.address.city }}
          </h1>
        </div>
      </RouterLink>
    </div>
  </div>
</template>
