
<script setup lang="ts">
import { computed, ref } from 'vue';

const working = "WORKING"

const tipValues = ref({
    5: 5, 10: 10, 15: 15, 25: 25, 50: 50
})

const selectedTipRatio = ref<number>(0)
const numberOfPeople = ref<number>(0)
const billAmount = ref<number>(0)
const customTipAmount = ref<number | undefined>()

const checkIsNumberAreDisplayable = (number: number) => Number.isNaN(number) || !Number.isFinite(number) ? 0 : Number(number.toFixed(2));

const handleSelectTipRation = (tipRatio: number) => {
    customTipAmount.value = undefined
    selectedTipRatio.value = tipRatio
}

const handleCustomTipAmountChange = (e: Event) => {
    const target = e.target as HTMLInputElement
    handleSelectTipRation(0)
    customTipAmount.value = Number(target.value)
}

const handleResetForm = () => {
    selectedTipRatio.value = 0
    numberOfPeople.value = 0
    billAmount.value = 0
    customTipAmount.value = undefined

}


const tipAmountPerPerson = computed({
    get: () =>{
        const tipRatio = customTipAmount.value !== undefined ? customTipAmount.value : selectedTipRatio.value
        const tipPercent = (tipRatio / 100);
        const tipValue = billAmount.value * tipPercent;
        const result = Number(tipValue / numberOfPeople.value)
        
        
        return checkIsNumberAreDisplayable(result)
  },
    set: (newVal) => (newVal)
})

const  totalAmountPerPerson = computed({
    get: () => {
        const result =  ((billAmount.value + tipAmountPerPerson.value) / numberOfPeople.value)
        return checkIsNumberAreDisplayable(result)
},
    set: (newValue) => newValue,
})

const colors = {
    veryDarkCyan: 'hsl(183, 100%, 15%)',
    darkGrayishCyan: 'hsl(186, 14%, 43%)',
    grayishCyan: 'hsl(184, 14%, 56%)',
    lightGrayishCyan: 'hsl(185, 41%, 84%)',
    veryLightGrayishCyan: 'hsl(189, 41%, 97%)',
    white: 'hsl(0, 0%, 100%)',
}
</script>

<template>
    <section class="rounded-3xl p-8 shadow border grid grid-cols-2 gap-16">
        <div>
            <label for="bill" class="flex flex-col">
                <span>Bill:</span>
                <input type="number" v-model="billAmount" class="rounded border" name="bill" id="bill">
            </label>
            <div class="text-white">
                <p class="text-teal-800">Select tip %</p>
                <div class="flex flex-wrap gap-2">
                    <template v-for="(amount, key) in tipValues">
                        <button 
                            class="bg-[#00474b] rounded w-1/3 p-2"
                            :class="{'bg-[#27C3AE]': amount === selectedTipRatio }"
                            @click="() => handleSelectTipRation(amount)"
                        >
                            {{ amount }}
                        </button>
                    </template>
                    <input :value="customTipAmount" @input="(e) => handleCustomTipAmountChange(e)" class="w-1/3 text-black border" placeholder="Custom" type="text">
                </div>
                
            </div>
            <label for="noOfPeople">Number of People
                <input v-model="numberOfPeople"  type="number" class="rounded border" name="noOfPeople" id="noOfPeople">
            </label>
        </div>
        <div class="rounded-xl bg-[#00474b] text-white p-4 flex flex-col justify-between">
            <div>
                <div class="flex justify-between">
                    <p>Tip amount <br><span>/person </span> </p>
                    <p>$ {{tipAmountPerPerson }}</p>
                </div>
                <div class="flex justify-between">
                    <p>Total <br><span>/person </span> </p>
                    <p>$ {{totalAmountPerPerson}}</p>
                </div>
            </div>
            <button @click="handleResetForm" class='w-full bg-teal-100 text-black uppercase p-2 self-end'> reset </button>
        </div>
    </section>
</template>

<style>
h1 {
    color: v-bind('colors.veryDarkCyan')
}
</style>