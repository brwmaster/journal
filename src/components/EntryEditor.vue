<script lang="ts" setup>
import { ref, computed, defineEmits } from "vue";
import EmojiField from "@/components/EmojiField.vue";
import type Emoji from "@/types/Emoji";
import type Entry from "@/types/Entry";
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg?component";

// data
const body = ref("");
const emoji = ref<Emoji | null>(null);
const maxChars = 280;

// computed
const charCount = computed(() => body.value.length);

// methods
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;

  if (textarea.value.length <= maxChars) {
    body.value = textarea.value;
  } else {
    body.value = textarea.value = textarea.value.substring(0, maxChars);
  }
};

// emits
defineEmits<{
  (e: "@create", entry: Entry): void;
}>();
</script>

<template>
  <form
    class="entry-form"
    @submit.prevent="
      $emit('@create', {
        id: Math.random(),
        body,
        emoji,
        createdAt: new Date(),
        userId: 1,
      })
    "
  >
    <textarea
      :value="body"
      @keyup="handleTextInput"
      placeholder="New Journal Entry for danielkelly_io"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
