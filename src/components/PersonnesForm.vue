<template>
    <form class="space-y-6" @submit.prevent="storePersonnes">
        <div>
            <label for="civility_id" class="block" >Civilité</label>

            <input type="radio" id="civility_id" v-model="form.civility_id" value="1" />
            <label for="civility_id" class="p-1" >Mme.</label>

            <input type="radio" v-model="form.civility_id" value="2" />
            <label for="civility_id" class="p-1">M.</label>

            <input type="radio" v-model="form.civility_id" value="3" />
            <label for="civility_id" class="p-1">Autre</label>
            
            
            
        </div>
        <div>
            <label for="last_name" class="block">Nom</label>
            <input type="text" id="last_name" v-model="form.last_name">
        </div>
        <div>
            <label for="first_name" class="block">Prénom</label>
            <input type="text" id="first_name" v-model="form.first_name">
        </div>
        <div>
            <label for="email" class="block">Email</label>
            <input type="text" id="email" v-model="form.email">
        </div>
        <div>
            <label for="phone" class="block">Téléphone</label>
            <input type="text" id="phone" v-model="form.phone">
        </div>
        <div>
            <label for="organisation_id" class="block">Entreprise</label>
            <input v-model="organisation_name" @input="changeOrganisation" >
            <select v-model="form.organisation_id" @change="()=>{organisation_name=''}">
                <option v-for="organisation in organisations" :value="organisation.id">
                {{ organisation.organisation_name }}
                </option>
            </select>
        </div>
        <div>
            <label for="location" class="block">Département(s) :</label>
            <select v-model="temp_location_id" @change="addLocation">
                <option value="" disabled selected>Choisissez une localisation</option>
                <option class="disabled:text-red" v-for="location in locations" :key="location.id" :value="location.id" :disabled="form.locations.filter((loc)=>loc.id==location.id).length" v-text="location.location_name"></option>
            </select>
            <div>
                
                <div v-for="location in form.locations" :key="location.id">
                    <p>{{location.location_name}}</p>
                    <div @click="removeLocation(location)"><p>Supprimer</p></div>
                </div>
                
            </div>
        </div>
        <button type="submit" :disabled="bool" class="bg-blue-500 px-2 py-1 text-white rounded">Créer la personne</button>
    </form>
</template>
<script setup>
import { reactive, onMounted, ref } from "vue";
import usePersonnes from "../services/personnesservices";
import useOrganisations from "../services/organisationsservices";
import useLocations from "../services/locationsservices";
import useCivilities from "../services/civilitiesservices";
import router from "../router";

        const temp_location_id = ref('')
        const form = reactive({
            last_name: '',
            first_name: '',
            email: '',
            phone: '',
            organisation_id: '',
            locations: [],
        });

        const bool = ref(false);

        const organisation_name = ref();

        const { createPersonnes } = usePersonnes();

        const { organisation, organisations, getOrganisations, getOrganisationsByName } = useOrganisations();

        const { location, locations, getLocations } = useLocations();

        const addLocation = () => {
            const searchedLocation = locations.value.filter((loc)=>loc.id==temp_location_id.value)[0]
            form.locations.push(searchedLocation)
            form.locations.sort((a,b)=>{return a.id - b.id})
            temp_location_id.value=''
        }

        const removeLocation = (loc) => {
            const index = form.locations.indexOf(loc)
            form.locations.splice(index, 1)
        }

        const storePersonnes = async () => {
            try {  
                bool.value = true;
                await createPersonnes({...form});
                router.push({name: 'people.list'});
            }
            catch (e) {
                bool.value = false;
            }
        };


        const changeOrganisation = async () => {
            organisations.value = await getOrganisationsByName(organisation_name.value)
        }

        const { civility, civilities, getCivilities } = useCivilities();
onMounted(()=>{getCivilities()});

        onMounted(async()=>{
             await getLocations()
        });

</script>