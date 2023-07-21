<script setup>
import { computed, unref } from 'vue'
import { useDisplay } from 'vuetify'

const display = useDisplay();

const isSmartPhone = computed(() => {
  return unref(display.xs)
});

const routes = [
  { to: '/home', text: 'Home' },
  { to: '/create', text: 'Add Devise' },
  { to: '/createPaire', text: 'Add Paire' }
]

const token = localStorage.getItem('token')

</script>
<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar title="Currency Converter" color="blue">
      <!-- Menu Mobile -->
    <v-menu v-if="isSmartPhone">
      <template v-slot:activator="{ props }">
        <v-app-bar-nav-icon v-bind="props" />
      </template>

      <v-list>
        <v-list-item v-for="{ to, text } in routes" :to="to" :key="to">
          <v-list-item-title>{{ text }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>

    <!-- Menu Desktop -->
    <v-toolbar-items v-else>
      <template v-for="{ to, text } in routes" :key="to">
        <v-btn :to="to" variant="text">{{ text }}</v-btn>
        <v-divider vertical></v-divider>
      </template>
    </v-toolbar-items>
    </v-app-bar>
    

    <v-main class="mt-5 align-center justify-center" style="min-height: 300px;">
      <router-view />
    </v-main>
  </v-layout>
</template>
