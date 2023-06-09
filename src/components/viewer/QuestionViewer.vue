<script setup>
import { ref } from "vue";

const { question, index, modelValue } = defineProps({
  question: Object,
  index: Number,
  modelValue: [String, Array],
});

const emits = defineEmits(["update:modelValue"]);

let model;

if (question.type === "checkbox") {
  model = ref({});
}

const onCheckboxChange = ($event) => {
  const selectedOptions = [];
  for (let text in model.value) {
    if (model.value[text]) {
      selectedOptions.push(text);
    }
  }
  emits("update:modelValue", selectedOptions);
};
</script>

<template>
  <fieldset class="mb-4">
    <div>
      <legend class="text-base font-medium text-gray-900">
        {{ index + 1 }}. {{ question.question }}
      </legend>
      <p class="text-gray-500 text-sm">{{ question.description }}</p>
    </div>

    <div class="mt-3">
      <div v-if="question.type === 'select'">
        <select
          :value="modelValue"
          @change="emits('update:modelValue', $event.target.value)"
          class="mt-2 text-black block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
        >
          <option value="">Please select</option>
          <option
            :value="option.text"
            v-for="option in question.data.options"
            :key="option.uuid"
          >
            {{ option.text }}
          </option>
        </select>
      </div>

      <div v-else-if="question.type === 'radio'">
        <div
          v-for="(option, index) in question.data.options"
          :key="option.uuid"
          class="flex items-center mt-2"
        >
          <input
            type="radio"
            :id="option.uuid"
            :value="option.text"
            :name="'question' + question.id"
            @change="emits('update:modelValue', $event.target.value)"
            class="focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-300"
          />
          <label
            :for="option.uuid"
            class="ml-3 block text-sm font-medium text-gray-700"
            >{{ option.text }}</label
          >
        </div>
      </div>

      <div v-else-if="question.type === 'checkbox'">
        <div
          v-for="(option, index) of question.data.options"
          :key="option.uuid"
          class="flex items-center"
        >
          <input
            type="checkbox"
            :id="option.uuid"
            v-model="model[option.text]"
            @change="onCheckboxChange"
            class="my-2 focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-300 rounded"
          />
          <label
            :for="option"
            class="ml-3 block text-sm font-medium text-gray-700"
            >{{ option.text }}</label
          >
        </div>
      </div>

      <div v-else-if="question.type === 'text'">
        <input
          type="text"
          :value="modelValue"
          @input="emits('update:modelValue', $event.target.value)"
          class="mt-2 text-black focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
        />
      </div>

      <div v-else="question.type === 'textarea'">
        <textarea
          :value="modelValue"
          @input="emits('update:modelValue', $event.target.value)"
          class="mt-2 text-black focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
        ></textarea>
      </div>
    </div>

    <hr class="my-8" />
  </fieldset>
</template>

<style lang="scss" scoped></style>
