<script setup lang="ts">
import { ref } from 'vue';

const activeteam_time_remaining = ref("0:00");
const radiant_bonus_time = ref("0:00");
const dire_bonus_time = ref("0:00");
</script>

<template>
    <div class="relative flex flex-col w-[1280px] bg-blue-300">
        <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
            <div class="flex flex-row mb-12">

                <img class="h-14 scale-x-[-1]" src="../assets/images/arrow.png" alt="" />


                <p class="text-5xl activeteam_time_remaining text-black">
                    {{ activeteam_time_remaining }}
                </p>

                <img class="h-14 opacity-0" src="../assets/images/arrow.png" alt="" />

            </div>
        </div>
        <div class="absolute bottom-0 w-full bg-orange-500">
            <div class="flex flex-row">
                <div class="w-full bg-emerald-400">
                    <div class="flex flex-col pl-2">
                        <p class="text-sm reserve_time text-black">RESERVE TIME</p>
                        <p class="text-2xl reserve_time text-black">{{ radiant_bonus_time }}</p>
                    </div>
                </div>
                <div class="w-full bg-red-500">
                    <div class="flex flex-col items-end pr-2">
                        <p class="text-sm reserve_time text-black">RESERVE TIME</p>
                        <p class="text-2xl reserve_time text-black">{{ dire_bonus_time }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>