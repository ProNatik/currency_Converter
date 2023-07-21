<script setup>
import { useTitle } from '@vueuse/core';
import { ref, reactive } from 'vue';
import axiosClient from '../axios';
import { usePaires } from '@/composables/paires.js'
import { paireRemove } from '@/services/devises.js'

useTitle('Home - CurrencyConverter')

const form = reactive({
  from_devise: ref(''),
  to_devise: ref(''),
  value: ref(''),
});

const errorMessage = ref(null);
const success = ref(null);

const { paires, error, loading } = usePaires()

async function deletePaire(paire) {
  errorMessage.value = null
  try {
    await paireRemove(paire)
    paires.value = paires.value.filter((p) => p !== paire)
  } catch (error) {
    errorMessage.value = error.message
  }
}

</script>

<template>
    <v-alert type="error" v-if="errorMessage" :text="errorMessage" />
  <v-table theme="dark" v-if="paires" max-width="800">
    <thead>
      <tr>
        <th class="text-left">
          Paires
        </th>
        <th class="text-left">
          Converter
        </th>
        <th class="text-left">
          Delete
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="paire in paires"
        :key="paire.created_at"
      >
        <td>{{ paire.from_devise }} / {{ paire.to_devise }}</td>
        <td>{{ paire.value }} </td>
        <td>
            <v-btn
            @click="deletePaire(paire)"
            variant="outlined"
            size="x-small"
            color="teal"
            >Supprimer</v-btn
          >
        </td>
      </tr>
    </tbody>
  </v-table>
  <v-skeleton-loader
      type="table"
      v-else-if="loading"
      :style="{ maxWidth: '30rem' }"
      class="ma-auto"
    />
</template>