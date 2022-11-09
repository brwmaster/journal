<script setup lang="ts">
import { inject, provide, reactive } from "vue";
import { userInjectionKey } from "./injectionKeys";
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import type User from "./types/User";
import type Entry from "./types/Entry";

const entries: Entry[] = reactive([]);

const user: User = reactive({
  id: 1,
  username: "edwinboon_dev",
  settings: [],
});

provide(userInjectionKey, user);

const handleCreateEntry = (entry: Entry) => {
  entries.unshift(entry);
};
</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry($event)" />
    <ul>
      <li v-for="entry in entries" :key="entry.id">
        <EntryCard :entry="entry" />
      </li>
    </ul>
  </main>
</template>
