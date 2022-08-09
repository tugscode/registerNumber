<script setup lang="ts">
import { reactive } from "vue";

import { useTranslation } from "i18next-vue";
// Vuelidate
import useVuelidate from "@vuelidate/core";
import { required, helpers } from "@vuelidate/validators";

// Components
import LetterPicker from "./LetterPicker.vue";

const { t, i18next } = useTranslation();

const emit = defineEmits(["submit"]);

const props = defineProps(["loading"]);

let state = reactive({
  firstLetter: "А",
  secondLetter: "А",
  number: null,
});

const rules = {
  number: {
    required: helpers.withMessage(
      t("insert-correct-register-number", { lang: i18next.language }),
      required
    ),
  },
};

const validator = useVuelidate(rules, state);

async function submitForm() {
  const result = await validator.value.$validate();

  if (result) {
    emit("submit", state.firstLetter + state.secondLetter + state.number);
  }
}

function isNumber(event: any) {
  if (event.target.value.length < 8) {
    var charCode = event.which ? event.which : event.keyCode;

    if (charCode > 31 && (charCode < 48 || charCode > 57) && charCode !== 46) {
      event.preventDefault();
    }
  } else {
    event.preventDefault();
  }
}

function changeFirstLetter(value: string) {
  state.firstLetter = value;
}

function changeSecondLetter(value: string) {
  state.secondLetter = value;
}
</script>

<template>
  <form @submit.prevent="submitForm" class="flex space-x-5">
    <LetterPicker @change-letter="changeFirstLetter" />
    <LetterPicker @change-letter="changeSecondLetter" />
    <div>
      <input
        type="text"
        :disabled="loading"
        class="h-10 px-5 border rounded-full w-full text-sm border-gray-600 text-[16px] font-bold tracking-widest"
        :placeholder="$t('insert-register-number')"
        v-model="state.number"
        @keypress="isNumber($event)"
      />
      <div
        class="flex flex-col space-y-4 pl-5 pt-1"
        v-for="error of validator.number.$errors"
        :key="error.$uid"
      >
        <div class="text-xs italic text-red-600">{{ error.$message }}</div>
      </div>
    </div>
    <button
      type="submit"
      :disabled="loading"
      class="h-10 w-10 rounded-full flex flex-shrink-0"
      :class="loading ? 'bg-blue-200' : 'bg-blue-300'"
    >
      <div v-if="loading" class="h-5 w-5 m-auto text-white">
        <i class="pi pi-spin pi-spinner"></i>
      </div>
      <svg
        v-else
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 20 20"
        fill="currentColor"
        aria-hidden="true"
        class="h-5 w-5 m-auto text-white"
      >
        <path
          fill-rule="evenodd"
          d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
          clip-rule="evenodd"
        ></path>
      </svg>
    </button>
  </form>
</template>
