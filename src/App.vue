<script setup>
    import { ref, computed , watch} from 'vue';
    import Header from './components/Header.vue'
    import Button from './components/Button.vue'
    import {totalPagar} from './helpers';

    const cantidad = ref(0);
    const meses = ref(6);
    const total = ref(0)

    const MIN = 0;
    const MAX = 20000;
    const STEP = 100;
    
    const formatearDinero= (valor)=>{
        const formatter= new Intl.NumberFormat('en-US', {
            style: 'currency',

            currency: 'USD',
        })
        return formatter.format(valor)
    };

    watch ([cantidad, meses], () =>{
        total.value = totalPagar(cantidad.value, meses.value)
    }, {immediate:true});

    const handleChangeDecremento = ()=>{

        const valor = cantidad.value - STEP

        if(valor < MIN){
            alert('Limite alcanzado');
            return;
        }

        cantidad.value = valor;
        
    };

    const handleChangeIncremento = ()=>{
        const valor = cantidad.value + STEP;

        if(valor > MAX ){
            alert('Limite alcanzado');
            return;
        };

        cantidad.value = valor;
    };

    const totalpago = computed(()=>{
        return total.value / meses.value;
    });

    // const state = reactive({
    //     cantidad: 0
    // })

    // function handleChange(e){
    //     cantidad.value = +e.target.value;
    // }
    
</script>

<template>

    <div class="my-20 mx-auto max-w-lg bg-white shadow p-10 ">
        <Header />
        
        <div class="flex justify-between mt-10">
            <Button 
            :operador="'-'"
            @fn="handleChangeDecremento"
            />
            <Button 
            :operador="'+'"
            @fn="handleChangeIncremento"
            />
        </div>

        <div class="my-5">
            <input 
                type="range" 
                class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
                :min="MIN"
                :max="MAX"
                :step="STEP"
                v-model.number="cantidad"
            />

            <p class="text-center mt-5 text-4xl font-extrabold text-indigo-600">{{formatearDinero(cantidad)}}</p> 
            <h2 class="text-2xl foont-extrabold text-gray-500 text-center">Elige un <span class="text-indigo-600"> plazo</span> a pagar</h2>
            <select 
            class="mt-5 w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500"
            :value="meses"
            v-model.number="meses">
                <option value="6">6 meses</option>
                <option value="12">12 meses</option>
                <option value="18">18 meses</option>
            </select>
        </div>
        <div v-if="total > 0" class="my-5 space-y-3 bg-gray-50 p-5">
            <h2 class="text-center font-extrabold text-gray-500 text-2xl">
                Resumen <span class="text-indigo-600">de pagos</span>
            </h2>
            <p class="text-xl text-center font-bold text-gray-500">{{ meses }} Meses</p>
            <p class="text-xl text-center font-bold text-gray-500">Total a pagar: {{ formatearDinero(total)}}</p>
            <p class="text-xl text-center font-bold text-gray-500">Mensuales: {{ formatearDinero(totalpago)}} </p>

        </div>
        <p v-else class="text-center font-bold">Añade una cantidad y un plazo para calcular el valor a pagar</p>
    </div>
  
</template>

