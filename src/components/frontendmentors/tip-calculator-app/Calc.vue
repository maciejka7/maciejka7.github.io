
<script setup lang="ts">
import { ref } from 'vue';

const working = "WORKING"

const tipValues = ref({
    5: 5, 10: 10, 15: 15, 25: 25, 50: 50, custom: 0
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
            <label for="bill">bill
                <input type="number" class="rounded border" name="bill" id="bill">
            </label>

            <div class="text-white">
                <p>Select tip %</p>
                <div class="flex flex-wrap gap-2">
                    <template v-for="(amount, key) in tipValues">
                        <button class="bg-[#00474b] rounded w-1/3 p-2" v-if="key !== 'custom'">
                            {{ amount }}
                        </button>
                        <input class="w-1/3 text-black border" v-else type="text">
                    </template>
                </div>
            </div>
            <label for="noPeople">Number of People
                <input type="number" class="rounded border" name="noPeople" id="noPeople">
            </label>
        </div>
        <div class="rounded-xl bg-[#00474b] text-white">
            <div>
                <p>Tip amount </p>
                <p>/person </p>
                <p>$0.00</p>
            </div>
            <div>
                <p>Total </p>
                <p>/person </p>
                <p>$0.00</p>
            </div>

            <button> reset </button>
        </div>
    </section>
</template>

<style>
h1 {
    color: v-bind('colors.veryDarkCyan')
}
</style>