<template>
  <PageComponent title="Dashboard">
    <div v-if="loading">Loading...</div>
    <div
      v-else
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 text-gray-700 animate-fade-in-down"
    >
      <div
        class="bg-white shadow-md p-3 text-center flex flex-col order-1 lg:order-2 animate-fade-in-down"
        style="animation-delay: 0.1s"
      >
        <h3 class="text-2xl font-semibold">Total Surveys</h3>
        <div
          class="text-8xl font-semibold flex flex-1 items-center justify-center"
        >
          {{ data.totalSurveys }}
        </div>
      </div>

      <div
        class="bg-white shadow-md p-3 text-center flex flex-col order-1 lg:order-4 animate-fade-in-down"
        style="animation-delay: 0.2s"
      >
        <h3 class="text-2xl font-semibold">Total Answers</h3>
        <div
          class="text-8xl font-semibold flex flex-1 items-center justify-center"
        >
          {{ data.totalAnswers }}
        </div>
      </div>

      <div
        class="row-span-2 order-3 lg:order-1 bg-white shadow-md p-4 animate-fade-in-down"
      >
        <h3 class="text-2xl font-semibold">Latest Answers</h3>
        <img
          :src="data.latestSurvey.image_url"
          class="w-[240px] mx-auto"
          alt="Lastest Answer"
        />
        <h3 class="font-bold text-xl mb-3">{{ data.latestSurvey.title }}</h3>
        <div class="flex justify-between text-sm mb-1">
          <div>Create date:</div>
          <div>{{ data.latestSurvey.created_at }}</div>
        </div>
        <div class="flex justify-between text-sm mb-1">
          <div>Expire date:</div>
          <div>{{ data.latestSurvey.expire_date }}</div>
        </div>
        <div class="flex justify-between text-sm mb-1">
          <div>Status:</div>
          <div>{{ data.latestSurvey.status ? "Active" : "Draft" }}</div>
        </div>
        <div class="flex justify-between text-sm mb-1">
          <div>Questions:</div>
          <div>{{ data.latestSurvey.questions }}</div>
        </div>
        <div class="flex justify-between text-sm mb-3">
          <div>Answers:</div>
          <div c>{{ data.latestSurvey.answers }}</div>
        </div>
        <div class="flex justify-between">
          <router-link
            :to="{ name: 'SurveyView', params: { id: data.latestSurvey.id } }"
            class="flex py-2 px-4 border border-transparent text-sm rounded-md text-indigo-500 hover:bg-indigo-700 hover:text-white transition-colors focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5 h-5 mr-2"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L10.582 16.07a4.5 4.5 0 01-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 011.13-1.897l8.932-8.931zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0115.75 21H5.25A2.25 2.25 0 013 18.75V8.25A2.25 2.25 0 015.25 6H10"
              />
            </svg>

            Edit survey
          </router-link>

          <button
            class="flex py-2 px-4 border border-transparent text-sm rounded-md text-indigo-500 hover:bg-indigo-700 hover:text-white transition-colors focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5 h-5 mr-2"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M8.625 9.75a.375.375 0 11-.75 0 .375.375 0 01.75 0zm0 0H8.25m4.125 0a.375.375 0 11-.75 0 .375.375 0 01.75 0zm0 0H12m4.125 0a.375.375 0 11-.75 0 .375.375 0 01.75 0zm0 0h-.375m-13.5 3.01c0 1.6 1.123 2.994 2.707 3.227 1.087.16 2.185.283 3.293.369V21l4.184-4.183a1.14 1.14 0 01.778-.332 48.294 48.294 0 005.83-.498c1.585-.233 2.708-1.626 2.708-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0012 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018z"
              />
            </svg>

            View Answers
          </button>
        </div>
      </div>

      <div
        class="bg-white shadow-md p-3 row-span-2 order-4 lg:order-3"
        style="animation-delay: 0.3s"
      >
        <div class="flex justify-between items-center mb-3 px-2">
          <h3 class="text-2xl font-semibold">Lastest Answers</h3>
          <a
            href="javascript:void(0)"
            class="text-sm text-blue-500 hover:decoration-blue-500"
            >View all</a
          >
        </div>
        <a
          href="#"
          v-for="answer of data.latestAnswers"
          :key="answer.id"
          class="block p-2 hover:bg-gray-100/90"
        >
          <div class="font-semibold">{{ answer.survey.title }}</div>
          <small
            >Answer made at:
            <i class="font-semibold">{{ answer.end_date }}</i>
          </small>
        </a>
      </div>
    </div>
  </PageComponent>
</template>
<script setup>
import PageComponent from "../components/PageComponent.vue";
import { useStore } from "vuex";
import { computed } from "vue";

const store = useStore();

const loading = computed(() => store.state.dashboard.loading);
const data = computed(() => store.state.dashboard.data);

store.dispatch("getDashboardData");
</script>

<style lang="scss" scoped></style>
