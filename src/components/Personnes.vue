<template>

<div class="m-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 display: inline-block">

    <div class="flex flex-col p-5">
        <h4 class="mb-1 text-xl font-medium text-gray-900 dark:text-white" v-if="person.civility">{{ person.civility.gender}}</h4>
        <h5 class="mb-1 text-2xl font-medium text-gray-900 dark:text-white">{{ person.last_name }} {{ person.first_name }}</h5>
        <h5 class="mb-1 text-l font-medium text-gray-900 dark:text-white" v-if="person.organisation" >Entreprise : <router-link :to="{ name: 'organisations.details', params: { id: person.organisation.id }}">{{ person.organisation.organisation_name}}</router-link></h5>
        <ul>Département(s) :
            <li v-for="location in person.locations" :key="location.id" v-if="person.locations"> {{ location.location_name }} </li>
        </ul>
        <span class="text-sm text-gray-500 dark:text-gray-400">{{person.email}}</span>
        <span class="text-sm text-gray-500 dark:text-gray-400">{{person.phone}}</span>
        <div class="flex mt-4 space-x-3 md:mt-6">
            <router-link :to="{ name: 'people.edit', params: { id:person.id }}" class="inline-flex items-center px-4 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Modifier</router-link>
            <button @click="destroyPersonne(person.id)" class="inline-flex items-center px-4 py-2 text-sm font-medium text-center text-gray-900 bg-white border border-gray-300 rounded-lg hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-gray-200 dark:bg-gray-800 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-700 dark:focus:ring-gray-700">Supprimer</button>
        </div>
    </div>
</div>


</template>
<script setup>
    import { onMounted } from "vue";
    import usePersonnes from "../services/personnesservices";

    const props = defineProps({
        id: {
            required: true,
            type: String
        }
    }) 

    const { getPersonne, person, destroyPersonne } = usePersonnes();


    onMounted(()=>{getPersonne(props.id)});

</script>