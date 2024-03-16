<script setup lang="ts">
import { ref } from 'vue';
import HeroPick from '@/components/HeroPick.vue';
import Heroban from '@/components/Heroban.vue';
import CenterScreen from '@/components/CenterScreen.vue';

const readiantPicks = ref([
  {
    id: 0,
    name: 'abaddon',
  },
  {
    id: 1,
    name: 'antimage',
  },
  {
    id: 2,
    name: 'crystal_maiden',
  },
  {
    id: 3,
    name: 'drow_ranger',
  },
  {
    id: 4,
    name: 'none',
  }
])

const direPicks = ref([
  {
    id: 0,
    name: 'invoker',
  },
  {
    id: 1,
    name: 'lycan',
  },
  {
    id: 2,
    name: 'phoenix',
  },
  {
    id: 3,
    name: 'medusa',
  },
  {
    id: 4,
    name: 'nevermore',
  }
])

const readiantBans = ref([
  {
    id: 0,
    name: 'abaddon',
  },
  {
    id: 1,
    name: 'axe',
  },
  {
    id: 2,
    name: 'crystal_maiden',
  },
  {
    id: 3,
    name: 'banning',
  },
  {
    id: 4,
    name: 'none',
  },
  {
    id: 5,
    name: 'none',
  },
  {
    id: 6,
    name: 'none',
  },
  {
    id: 7,
    name: 'none',
  },
  {
    id: 8,
    name: 'none',
  },

])
const direBans = ref([
  {
    id: 0,
    name: 'abaddon',
  },
  {
    id: 1,
    name: 'axe',
  },
  {
    id: 2,
    name: 'crystal_maiden',
  },
  {
    id: 3,
    name: 'banning',
  },
  {
    id: 4,
    name: 'none',
  },
  {
    id: 5,
    name: 'none',
  },
  {
    id: 6,
    name: 'none',
  },
  {
    id: 7,
    name: 'none',
  },
  {
    id: 8,
    name: 'none',
  },

])

</script>

<template>


  <div class="relative h-screen w-screen">
    <div class="absolute bottom-0 w-full h-[500px] flex items-end">
      <div class="flex flex-row w-full px-3 pb-3">
        <div class="flex flex-row w-full">
          <div class="flex flex-col w-full">
            <div class="flex flex-row">

              <div v-for="pick in readiantPicks" :key="pick.id">
                <HeroPick :hero_name="pick.name" />
              </div>

            </div>
            <div class="flex flex-row bg-red-600">

              <div v-for="pick in readiantBans" :key="pick.id">
                <Heroban :hero_name="pick.name" />
              </div>

            </div>
          </div>

          <CenterScreen />

          <div class="flex flex-col w-full">
            <div class="flex flex-row-reverse">

              <div v-for="pick in direPicks" :key="pick.id">
                <HeroPick :hero_name="pick.name" />
              </div>

            </div>
            <div class="flex flex-row-reverse bg-red-600">
              <div v-for="pick in direBans" :key="pick.id">
                <Heroban :hero_name="pick.name" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>



</template>
<style scoped></style>