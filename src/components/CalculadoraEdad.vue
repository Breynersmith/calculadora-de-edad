<template>
    <div class="flex flex-col gap-4 justify-arround items-center bg-white w-[90%] h-[80vh] mx-auto mt-16 max-w-[450px] rounded-br-[10rem] ">
        <p 
        v-if="error"
        class="text-red-500 text-xs py-1 -mb-12">Todos los campos son obligatorios</p>
        <form action="">
            <div class="flex gap-10 p-8 justify-end items-end">
            <label for="dia">
                <p>Dia</p>
                <input
                    :class="error ? 'error' : ''"
                    class="w-[40px]"
                    type="number" 
                    id="dia" 
                    placeholder="24"
                    min="1"
                    max="31"
                    v-model="dia"
                    required>
            </label>

            <label 
                for="meses">
                <p>Mes</p>
                <input 
                    :class="error ? 'error' : ''"
                    class="w-[40px]"
                    type="number" 
                    id="meses" 
                    placeholder="12"
                    min="1"
                    max="12"
                    v-model="mes"
                    required>
            </label>

            <label for="anio">
                <p>Año</p>
                <input 
                    :class="error ? 'error' : ''"
                    class="w-[70px]"
                    type="number" 
                    id="anio" 
                    placeholder="1994"
                    v-model="anio"
                    min="1900"
                    max="2024"
                    required>
            </label>
        </div>
        <div class="flex flex-col gap-4 justify-center items-center">
           
            <button 
                @click="calcular"
                class="bg-[#854EFF] p-1 rounded-full w-12 h-12 hover:bg-slate-700">
                    <img
                        class="w-[80%] mx-auto" 
                        src="/images/icon-arrow.svg" alt="">
            </button>
        </div>
        </form>
    
    <div class="text-5xl font-bold italic mb-10 text-center">
        <p><span class="text-[#854EFF] text-[4rem]">{{ dias }}</span>Dias</p>
        <p><span class="text-[#854EFF] text-[4rem]">{{ meses }}</span>Meses</p>
        <p><span class="text-[#854EFF] text-[4rem]">{{ anios }}</span>Años</p>
    </div>
</div>   
   
    
</template>

<script setup>
import { ref } from 'vue';

const dia = ref(null);
const mes = ref(null);
const anio = ref(null);

const dias = ref('--')
const meses = ref('--')
const anios = ref('--')

const error = ref(false)

const validarFormulario = () => {
    if (dia.value <= 0 || mes.value <= 0 || anio.value <= 1900 || anio.value > 2024) {
        error.value = true    
        setTimeout(() => {
            error.value = false
        },5000)
        return false;
    }
    return true;
}


const calcular = (e) => {
    e.preventDefault();
    if (validarFormulario() === true) {
        let fechaNacimiento = new Date(anio.value, mes.value - 1, dia.value);
    let fechaActual = new Date();
    let edad = fechaActual.getFullYear() - fechaNacimiento.getFullYear();
    let m = fechaActual.getMonth() - fechaNacimiento.getMonth();
    let d = fechaActual.getDate() - fechaNacimiento.getDate();

    if (m < 0 || (m === 0 && d < 0)) {
        edad--;
    }
    if (m < 0) m += 12;
    if (d < 0) {
        m--;
        d += 30;
    }

    anios.value = edad;
    meses.value = m;
    dias.value = d;
    }
    
}

</script>

<style scoped>  

label {
    padding: 5px;
    width: 80%;
    text-align: left;
    font-size: 12px;
    font-weight: bold;
    color: #bcbaba;
}


label input {
    font-weight: bold;
    color: black;
    border: 1px solid #d3d3d3;
    font-size: 18px;
}

label input:focus {
    outline: none;
    border: 1px solid #864eff67;
}

.error {
    box-shadow: 0 0 3px rgb(214, 61, 61);
    transition: all 0.2s ease;
}

</style>