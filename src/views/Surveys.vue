<script setup>
import PageComponent from "../components/PageComponent.vue";
import SurveyListItem from "../components/SurveyListItem.vue";
import store from "../store";
import { computed } from "vue";

const surveys = computed(() => store.state.surveys);

store.dispatch("getSurveys");

const deleteSurvey = (survey) => {
  if (
    confirm(
      `Are you sure you want to delete this survey? Operation cannnot be undone!`
    )
  ) {
    store
      .dispatch("deleteSurvey", survey.id)
      .then(() => store.dispatch("getSurveys"));
  }
};

const getForPage = (e, link) => {
  e.preventDefault();
  if (!link.url || link.active) {
    return;
  }
  store.dispatch("getSurveys", { url: link.url });
};
</script>

<template>
  <PageComponent>
    <template v-slot:header>
      <div class="flex justify-between items-center">
        <h1 class="text-3xl font-bold text-gray-900">Surveys</h1>
        <router-link
          to="/surveys/create"
          class="flex py-2 px-3 text-white bg-emerald-500 rounded-md hover:bg-emerald-600 hover:text-white"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4.5v15m7.5-7.5h-15"
            />
          </svg>
          Add new survey
        </router-link>
      </div>
    </template>

    <div v-if="surveys.loading" class="flex justify-center">Loading...</div>

    <div v-else>
      <div class="grid grid-cols-1 gap-3 sm:grid-cols-2 md:grid-cols-3">
        <SurveyListItem
          v-for="(survey, index) in surveys.data"
          :key="survey.id"
          :survey="survey"
          class="opacity-0 animate-fade-in-down"
          :style="{ animationDelay: `${index * 0.1}s` }"
          @delete="deleteSurvey(survey)"
        />
      </div>
      <div class="flex justify-center mt-5">
        <nav
          class="relative z-0 inline-flex justify-center rounded-md shadow-sm border"
          aria-label="pagination"
        >
          <a
            v-for="(link, i) of surveys.links"
            :key="i"
            :disabled="!link.url"
            href="#"
            @click="getForPage($event, link)"
            aria-current="page"
            v-html="link.label"
            class="relative inline-flex items-center px-4 py-2 broder text-sm font-medium whitespace-no-wrap"
            :class="[
              link.active
                ? 'z-10 bg-indigo-50 border-indigo-500 text-indigo-600'
                : 'bg-white border-gray-300 text-gray-500 hover:bg-gray-50',
              i === 0 ? 'rounded-l-md' : '',
              i === surveys.links.length - 1 ? 'rounded-r-md' : '',
            ]"
          ></a>
        </nav>
      </div>
    </div>
  </PageComponent>
</template>

<style lang="scss" scoped></style>
