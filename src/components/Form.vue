<script setup>
   import {reactive, computed} from 'vue';
   import VAlertOwner from './Alerts/Alerts.vue';

   //Emits
   const emit = defineEmits(['update:name', 'update:owner', 
                'update:email', 'update:high', 
                'update:symptoms', 'save-pet']);

   //Acción para guardar pacientes 
   const props = defineProps({
      id: {
         type: [String, null],
         required: true,
      },
      name:{
         type: String,
         required: true,
      },
      email:{
         type: String,
         required: true,
      },
      owner:{
         type: String,
         required: true,
      },
      high:{
         type: String,
         required: true,
      },
      symptoms:{
         type: String,
         required: true,
      },

   })

   //State del alerta variables
   const alert = reactive({
      type: '',
      msg: '',
   });

   //Methods
   const validator = () => {
      if (Object.values(props).includes('')) {
         alert.msg = 'Los campos son obligatorios'
         alert.type = 'error';
         return;
      }
      alert.msg = "Mascota registrada correctamente";
      alert.type = "success";
      emit('save-pet');
      setTimeout(() => {
         Object.assign(alert,{
            tipo: '',
            msg: '',
         }, 2000)
      })

      
   }

   //Computeds

   const edit = computed (() =>{
      return props.id;
   })

</script>

<template>
  <div class="md:w-1/2">
     <h2 class="font-black text-3xl text-center">Seguiento Paciente</h2>
     <p class="text-lg mt-5 text-center mb-10">
        Añade Pacientes y
        <span class="text-indigo-600 font-bold">Administralos</span>
     </p>
     <v-alert-owner
         class="mx-10"
         v-if="alert.msg"
         :alert="alert"
     />
     <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10 mx-10"  @submit.prevent="validator">
         <div class="mb-5">
            <label for="mascota"
               class="block text-grey-700 uppercase font-bold m-2"
            >
               Nombre de la mascota
            </label>
            <input id="mascota" type="text"
                  placeholder="Nombre de la mascota"
                  class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md"
                  :value="name"
                  @input="$emit('update:name', $event.target.value)"
            >
         </div>

         <div class="mb-5">
            <label for="propietario"
               class="block text-grey-700 uppercase font-bold m-2"
            >
               Nombre del propietario
            </label>
            <input id="propietario" type="text"
                  placeholder="Nombre del propietario"
                  class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md"
                  :value="owner"
                  @input="$emit('update:owner', $event.target.value)"
            >
         </div>

         <div class="mb-5">
            <label for="email"
               class="block text-grey-700 uppercase font-bold m-2"
            >
               Correo eléctronico
            </label>
            <input id="email" type="email"
                  placeholder="Correo eléctronico"
                  class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md"
                  :value="email"
                  @input="$emit('update:email', $event.target.value)"
            >
         </div>

         <div class="mb-5">
            <label for="alta"
               class="block text-grey-700 uppercase font-bold m-2"
            >
               Alta
            </label>
            <input id="alta" type="date"
                  placeholder="Alta"
                  class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md"
                  :value="high"
                  @input="$emit('update:high', $event.target.value)"

            />
         </div>

         <div class="mb-5">
            <label for="sintomas"
               class="block text-grey-700 uppercase font-bold m-2"
            >
               Sintomas
            </label>
            <textarea id="sintomas" type="text"
                  placeholder="Describe todos los sintomas"
                  class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md h-40"
                  :value="symptoms"
                  @input="$emit('update:symptoms', $event.target.value)"
            />
         </div>

         <input type="submit" class="bg-indigo-600 w-full p-2 m-2 text-white uppercase font-bold hover:bg-indigo-700 
            cursor-pointer" 
            :value="[edit ? 'Guardar cambios' :'Registrar paciente']"
         >
     </form>
  </div>
</template>