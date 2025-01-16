<script setup>
import { ref, onMounted } from "vue";
const users = ref([]);
const search = ref("");

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

onMounted(() => {
  getUsers();
});

const perfomSearch = () => {
  getUsers();
};
</script>

<template>
  <div class="flex items-center justify-between w-full p-4 text-xl font-bold text-black">
    <h1>Users</h1>
    <div>
      <input
        class="p-2 font-medium border rounded-lg text-md"
        id="search"
        name="search"
        type="text"
        placeholder="search by email"
        v-model="search"
        @keyup.enter="perfomSearch"
      />
      <button
        class="p-2 ml-4 font-medium text-white bg-blue-500 rounded-lg text-md"
        @click="perfomSearch"
      >
        Search
      </button>
    </div>
  </div>

  <div v-if="users.length <= 0" class="flex items-center justify-center h-96">
    <h1 class="text-2xl font-bold">Users is empty</h1>
  </div>

  <div class="gap-4">
    <div v-for="user in users" :key="user">
      <RouterLink :to="{ name: 'DetailsUser', params: { id: user.id } }" replace>
        <!-- <RouterLink :to="{ path:"/users/" + user.id}"> -->
        <div class="p-4 m-4 bg-red-200 rounded-lg shadow-lg">
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
