<script lang="ts" setup>
import { ref, computed } from "vue";
import EmojiField from "@/components/EmojiField.vue";
import type Emoji from "@/types/Emoji";
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg?component";

// data
const text = ref("");
const emoji = ref<Emoji | null>(null);
const maxChars = 280;

// computed
const charCount = computed(() => text.value.length);

// methods
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;

  if (textarea.value.length <= maxChars) {
    text.value = textarea.value;
  } else {
    text.value = textarea.value = textarea.value.substring(0, maxChars);
  }
};

// emits
defineEmits<{
  (e: "@create", entry: { text: string; emoji: Emoji | null }): void;
}>();
</script>

<template>
  <form class="entry-form" @submit.prevent="$emit('@create', { text, emoji })">
    <textarea
      :value="text"
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
