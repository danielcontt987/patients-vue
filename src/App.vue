<script setup>
  import { ref, reactive, onMounted, watch } from "vue";
  import { uid } from "uid";
  import Header from "./components/Header.vue";
  import Form from "./components/Form.vue";
  import VPet from "./components/Pet/Pet.vue";

  //Emits
  defineEmits(['update:email']);
  
  //Variables
  const pets = ref([]);

  const pet = reactive({
      id: null,
      name: '',
      owner: '',
      email: '',
      high: '',
      symptoms: '',
   });

  watch(pets, () => {
    saveLocalStorage()
  }, {
    deep: true
  })

  const saveLocalStorage = () => {
    localStorage.setItem('pets', JSON.stringify(pets.value))
  }

  onMounted(() => {
    const petStorage = localStorage.getItem('pets')
    if(petStorage) {
      pets.value = JSON.parse(petStorage)
    }
  })

  //Methods
  const savePet = () =>{
      if (pet.id) {
        const { id } = pet;
        const i = pets.value.findIndex((petState) => petState.id === id)
        pets.value[i] = {...pet}        
      }else{
        pets.value.push({
          ...pet,
          id: uid()
        });
      }
    
      pet.name = '';
      pet.owner = '';
      pet.email = '';
      pet.high = '';
      pet.symptoms = '';
      pet.id = null;
  }

  const updatePet = (id) => {
      const petEdit = pets.value.filter((pet) => pet.id === id)[0];
      Object.assign(pet, petEdit);
        
  }

  const deletePet = (id) => {
    const petDelete = pets.value.filter((pet) => pet.id !== id)[0];    
  }

</script>

<template>
   <div class="container mx-auto mt-20">
      <Header/>

      <div class="mt-12 md:flex">
        <Form
          v-model:id="pet.id"
          v-model:name="pet.name"
          v-model:owner="pet.owner"
          v-model:email="pet.email"
          v-model:high="pet.high"
          v-model:symptoms="pet.symptoms"
          @save-pet="savePet"
          :id="pet.id"
        />
        <div class="md:w-1/2 md:h-screen overflow-y-scroll">
           <h3 class="font-black text-3xl text-center">Administra tus paciente</h3>
           <div v-if="pets.length > 0">
            <p class="text-lg mt-5 text-center mb-10">
                Información de
                <span class="text-indigo-600 font-bold">pacientes</span>
            </p>
               <v-pet
                v-for="pet in pets"
                  :pet="pet"
                  :key="pet.name"
                  @update-pet="updatePet"
                  @delete-pet="deletePet"
               />
           </div>
           <p class="mt-20 text-2xl text-center" v-else>No hay pacientes</p>
        </div>
      </div>
    </div>
</template>