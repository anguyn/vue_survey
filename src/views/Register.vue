<script setup>
import { LockClosedIcon } from "@heroicons/vue/20/solid";
import store from "../store";
import { ref } from "vue";
import { useRouter } from "vue-router";
import Alert from "../components/Alert.vue";

const router = useRouter();

const errors = ref({});

const loading = ref(false);

const user = {
  name: "",
  email: "",
  password: "",
  password_confirmation: "",
};

const register = (e) => {
  e.preventDefault();
  loading.value = true;
  store
    .dispatch("register", user)
    .then((res) => {
      loading.value = false;

      router.push({ name: "Dashboard" });
    })
    .catch((e) => {
      loading.value = false;
      errors.value = e.response.data.errors;
    });
};
</script>

<template>
  <div>
    <img
      class="mx-auto h-12 w-auto"
      src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600"
      alt="Your Company"
    />
    <h2
      class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900"
    >
      Register for free
    </h2>
    <p class="mt-2 text-center text-sm text-gray-600">
      Or
      {{ " " }}
      <router-link
        to="/login"
        class="font-medium text-indigo-600 hover:text-indigo-500"
        >already have an account?</router-link
      >
    </p>
  </div>

  <form class="mt-8 space-y-6" @submit="register">
    <Alert
      v-if="Object.keys(errors).length"
      class="flex-col items-stretch text-sm"
    >
      <div v-for="(field, i) of Object.keys(errors)" :key="i">
        <div v-for="(error, ind) of errors[field] || []" :key="ind">
          *{{ error }}
        </div>
      </div>
    </Alert>
    <input type="hidden" name="remember" value="true" />
    <div class="-space-y-px rounded-md shadow-sm">
      <div>
        <label for="fullname" class="sr-only">Email address</label>
        <input
          id="fullname"
          name="fullname"
          type="text"
          required=""
          class="relative block w-full rounded-t-md border-0 py-1.5 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:z-10 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="Full name"
          v-model="user.name"
        />
      </div>
      <div>
        <label for="email-address" class="sr-only">Email address</label>
        <input
          id="email-address"
          name="email"
          type="email"
          autocomplete="email"
          required=""
          class="relative block w-full border-0 py-1.5 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:z-10 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="Email address"
          v-model="user.email"
        />
      </div>
      <div>
        <label for="password" class="sr-only">Password</label>
        <input
          id="password"
          name="password"
          type="password"
          autocomplete="current-password"
          required=""
          class="relative block w-full border-0 py-1.5 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:z-10 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="Password"
          v-model="user.password"
        />
      </div>
      <div>
        <label for="password_confirmation" class="sr-only"
          >Confirm password</label
        >
        <input
          id="password_confirmation"
          name="password_confirmation"
          type="password"
          autocomplete="current-password_confirmation"
          required=""
          class="relative block w-full rounded-b-md border-0 py-1.5 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:z-10 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="Confirm password"
          v-model="user.password_confirmation"
        />
      </div>
    </div>

    <div class="flex items-center justify-between">
      <div class="flex items-center">
        <input
          id="remember-me"
          name="remember-me"
          type="checkbox"
          class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-600"
        />
        <label for="remember-me" class="ml-2 block text-sm text-gray-900"
          >I read and accept SurveyAll policies and terms of use</label
        >
      </div>
    </div>

    <div>
      <button
        :disabled="loading"
        type="submit"
        class="group relative flex w-full justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        :class="{
          'cursor-not-allowed': loading,
          'hover:bg-indigo-500': loading,
        }"
      >
        <span class="absolute inset-y-0 left-0 flex items-center pl-3">
          <LockClosedIcon
            class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400"
            aria-hidden="true"
          />
        </span>
        <svg
          v-if="loading"
          aria-hidden="true"
          class="w-4 h-4 mr-3 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
          viewBox="0 0 100 101"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
            fill="currentColor"
          />
          <path
            d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
            fill="currentFill"
          />
        </svg>
        Sign Up
      </button>
    </div>
  </form>
</template>

<style scoped></style>
