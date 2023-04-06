<script setup>
import { ref, computed } from "vue";
import store from "../store";
import { v4 as uuidv4 } from "uuid";

const props = defineProps({
  question: Object,
  index: Number,
});

const emit = defineEmits(["change", "addQuestion", "deleteQuestion"]);

const questionTypes = computed(() => store.state.questionTypes);

const upperCaserFirst = (type) => {
  return type.charAt(0).toUpperCase() + type.slice(1);
};

const shouldHaveOptions = () => {
  return ["select", "radio", "checkbox"].includes(model.value.type);
};

const getOptions = () => {
  return model.value.data.options;
};

const setOptions = (options) => {
  model.value.data.options = options;
};

const addOption = () => {
  setOptions([...getOptions(), { uuid: uuidv4(), text: "" }]);
  dataChange();
};

const removeOption = (option) => {
  setOptions(getOptions().filter((opt) => opt !== option));
  dataChange();
};

const typeChange = () => {
  if (shouldHaveOptions()) {
    setOptions(getOptions() || []);
  }
  dataChange();
};

const dataChange = () => {
  const data = JSON.parse(JSON.stringify(model.value));
  if (!shouldHaveOptions()) {
    delete data.data.options;
  }

  emit("change", data);
};

const addQuestion = () => {
  emit("addQuestion", props.index + 1);
};

const deleteQuestion = () => {
  emit("deleteQuestion", props.question);
};

const model = ref(JSON.parse(JSON.stringify(props.question)));
</script>

<template>
  <div class="flex items-center justify-between">
    <h3 class="text-lg font-bold">{{ index + 1 }}. {{ model.question }}</h3>

    <div class="flex items-center">
      <!-- Add new question -->
      <button
        type="button"
        @click="addQuestion()"
        class="flex items-center text-xs py-1 px-3 mr-2 rounded-sm text-white bg-gray-600 hover:bg-gray-700"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-4 h-4"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 9v6m3-3H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
          />
        </svg>

        Add
      </button>
      <!--/ Add new question -->

      <!-- Delete question -->
      <button
        type="button"
        @click="deleteQuestion()"
        class="flex items-center text-xs py-1 px-3 rounded-sm border border-transparent text-red-500 hover:border-red-600"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-4 h-4"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 9.75L14.25 12m0 0l2.25 2.25M14.25 12l2.25-2.25M14.25 12L12 14.25m-2.58 4.92l-6.375-6.375a1.125 1.125 0 010-1.59L9.42 4.83c.211-.211.498-.33.796-.33H19.5a2.25 2.25 0 012.25 2.25v10.5a2.25 2.25 0 01-2.25 2.25h-9.284c-.298 0-.585-.119-.796-.33z"
          />
        </svg>

        Delete
      </button>
      <!--/ Delete question -->
    </div>
  </div>
  <div class="grid gap-3 grid-cols-12">
    <!-- Question -->
    <div class="mt-3 col-span-9">
      <label
        :for="'question_text_' + model.data"
        class="block text-sm font-medium text-gray-700"
      >
        Question Text
      </label>
      <input
        type="text"
        :name="'question_text_' + model.data"
        v-model="model.question"
        @change="dataChange"
        :id="'question_text_' + model.data"
        class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
      />
    </div>
    <!--/ Question -->

    <!-- Question type -->
    <div class="mt-3 col-span-3">
      <label
        for="question_type"
        class="block text-sm font-medium text-gray-700"
      >
        Select question type
      </label>
      <select
        name="question_type"
        id="question_type"
        v-model="model.type"
        @change="typeChange"
        class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
      >
        <option v-for="type in questionTypes" :key="type" :value="type">
          {{ upperCaserFirst(type) }}
        </option>
      </select>
    </div>

    <div class="mt-3 col-span-9">
      <label
        :for="'question_description_' + model.id"
        class="block text-sm font-medium text-gray-700"
      >
        Description
      </label>
      <textarea
        :name="'question_description_' + model.id"
        v-model="model.description"
        @change="dataChange"
        :id="'question_description_' + model.id"
        class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
      />
    </div>
  </div>

  <!-- Data -->
  <div>
    <div v-if="shouldHaveOptions()" class="mt-2">
      <h4 class="flex justify-between mb-4">
        Options
        <button
          type="button"
          @click="addOption()"
          class="flex items-center text-xs py-1 px-2 rounded-sm text-white bg-gray-600 hoveR:bg-gray-700"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-4 h-4"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4.5v15m7.5-7.5h-15"
            />
          </svg>
          Add options
        </button>
      </h4>

      <div
        v-if="!model.data.options.length"
        class="text-xs text-gray-600 text-center py-3"
      >
        You don't have any options defined
      </div>
      <div
        v-for="(option, index) in model.data.options"
        :key="option.uuid"
        class="flex items-center mb-1"
      >
        <span class="w-6 text-sm"> {{ index + 1 }}. </span>
        <input
          type="text"
          v-model="option.text"
          @change="dataChange"
          class="w-full rounded-sm py-1 px-2 text-xs border border-gray-300 focus:border-indigo-500"
        />
        <button
          type="button"
          @click="removeOption(option)"
          class="h-6 w-6 rounded-full flex items-center justify-center border border-transparent transition-colors hover:border-red-100"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="h-6 w-4 text-red-500"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M20.25 7.5l-.625 10.632a2.25 2.25 0 01-2.247 2.118H6.622a2.25 2.25 0 01-2.247-2.118L3.75 7.5m6 4.125l2.25 2.25m0 0l2.25 2.25M12 13.875l2.25-2.25M12 13.875l-2.25 2.25M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125z"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
  <!--/ Data -->

  <hr class="my-4" />
</template>

<style lang="scss" scoped></style>
