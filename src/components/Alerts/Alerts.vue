<script setup>
    import {computed} from 'vue'
    //props
    const props = defineProps({
        alert:{
            type: Object,
            required: true
        }
    });

    //computed
    const isError = computed(() => {
        return props.alert.type === "error";
    })
</script>

<template>
  <div 
    :class="[isError ? 'bg-red-600' : 'bg-green-600']"
     class="text-white text-center p-3 uppercase font-bold rounded-lg mb-5">
    {{ alert.msg }}
  </div>
</template>