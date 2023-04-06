<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="flex flex-center">
      <img alt="Quasar logo" src="~assets/logo.png" style="width: 80px" />
      <q-toolbar-title> DFCU API Simulator </q-toolbar-title>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "MainLayout",

  components: {},

  setup() {
    return {};
  },
});
</script>
