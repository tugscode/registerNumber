<script setup lang="ts">
import { ref, computed, watch } from "vue";
import {
  Listbox,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
} from "@headlessui/vue";

const letters = [
  "А",
  "Б",
  "В",
  "Г",
  "Д",
  "Е",
  "Ё",
  "Ж",
  "З",
  "И",
  "Й",
  "К",
  "Л",
  "М",
  "Н",
  "О",
  "Ө",
  "П",
  "Р",
  "С",
  "Т",
  "У",
  "Ү",
  "Ф",
  "Х",
  "Ц",
  "Ч",
  "Ш",
  "Щ",
  "Ъ",
  "Ы",
  "Ь",
  "Э",
  "Ю",
  "Я",
];

const emit = defineEmits(["changeLetter"]);

const query = ref("");

const letterValue = ref("А");

const filteredLetters = computed(() =>
  query.value === ""
    ? letters
    : letters.filter((letter) => {
        return letter.toLowerCase().includes(query.value.toLowerCase());
      })
);

watch(
  () => letterValue.value,
  (newValue) => {
    emit("changeLetter", newValue);
  }
);
</script>

<template>
  <Listbox v-model="letterValue">
    <div class="h-10 w-10 relative flex-shrink-0">
      <ListboxButton class="h-full w-full rounded-full border leter-font">{{
        letterValue
      }}</ListboxButton>
      <ListboxOptions
        class="w-[14rem] gap-[2px] absolute mt-5 bg-white border rounded-lg p-1 shadow-lg"
      >
        <input
          type="text"
          class="w-full py-2 px-4 text-sm leading-5 text-gray-900 mb-2 rounded-3xl border"
          placeholder="Хайх үсгээ оруулна уу"
          v-model="query"
        />
        <div class="focus:outline-none grid grid-cols-5 gap-[2px]">
          <ListboxOption
            v-for="letter in filteredLetters"
            :key="letter"
            :value="letter"
            v-slot="{ selected }"
          >
            <div
              :class="
                selected
                  ? 'bg-blue-400 text-white'
                  : 'bg-gray-100 text-gray-900'
              "
              class="relative w-10 h-10 rounded-full flex-shrink-0 border overflow-hidden cursor-pointer hover:bg-blue-400 hover:text-white hover:border-blue-400"
            >
              <span class="h-full w-full truncate flex font-normal"
                ><p class="m-auto">
                  {{ letter }}
                </p></span
              >
            </div>
          </ListboxOption>
        </div>
      </ListboxOptions>
    </div>
  </Listbox>
</template>

<style scoped>
.leter-font {
  --tw-border-opacity: 1;
  border-color: rgb(75 85 99 / var(--tw-border-opacity));
  font-weight: 700;
}
</style>
