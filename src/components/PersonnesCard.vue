<template>
<router-link :to="{name: 'people.list'}">
<div class="max-w-sm p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700">

        <h5 class="mb-2 text-2xl font-semibold tracking-tight text-gray-900 dark:text-white"  >Il y a {{ people.length }} personnes dans l'annuaire.</h5>

</div>
</router-link>
</template>

<script setup>
import usePersonnes from '../services/personnesservices';
import { onMounted } from "vue";

const { people, getPersonnes } = usePersonnes();
onMounted(async()=>{
        await getPersonnes()
    });

</script>
