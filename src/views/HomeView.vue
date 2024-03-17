<script setup lang="ts">
import { io } from 'socket.io-client';
import { ref } from 'vue';

import HeroPick from '@/components/HeroPick.vue';
import Heroban from '@/components/Heroban.vue';
import CenterScreen from '@/components/CenterScreen.vue';

const formatTime = (seconds: any) => {
  if (isNaN(seconds)) {
    return "0:00";
  }

  const minutes = Math.floor(seconds / 60);
  const remainingSeconds = seconds % 60;
  return `${minutes}:${remainingSeconds < 10 ? "0" : ""}${remainingSeconds}`;
}

const socket = io("http://localhost:3001");

const radiantState: any = ref([]);
const radiantPicks: any = ref([]);
const radiantBans: any = ref([]);

const direState: any = ref([]);
const direPicks: any = ref([]);
const direBans: any = ref([]);

const DIRE_BANS: any = ref([]);
const RADIANT_BANS: any = ref([]);
const DIRE_PICKS: any = ref([]);
const RADIANT_PICKS: any = ref([]);

const DRAFT_ACTIVE_TIME_REMAINING = ref();
const RADIANT_BONUS_TIME = ref();
const DIRE_BONUS_TIME = ref();
const RAW_DATA = ref();
const activeTeam = ref();

socket.on("newdata", (data) => {
  DRAFT_ACTIVE_TIME_REMAINING.value = data.draft.activeteam_time_remaining;
  RADIANT_BONUS_TIME.value = data.draft.radiant_bonus_time;
  DIRE_BONUS_TIME.value = data.draft.dire_bonus_time;
  RAW_DATA.value = JSON.stringify(data);

  activeTeam.value = data.draft.activeteam === 2 ? "radiant" : "dire";
  const isPickPhase = data.draft.pick;
  const phase = isPickPhase ? "picking" : "banning"; // Assign phase inside the callback



  for (let i = 0; i <= 6; i++) {
    const banKeyDire = `ban${i}_class`;
    const banDataDire = data.draft.team3[banKeyDire];
    const banKeyRadiant = `ban${i}_class`;
    const banDataRadiant = data.draft.team2[banKeyRadiant];

    if (banDataDire) {
      DIRE_BANS.value[i] = banDataDire;
      direState.value = banDataDire;
      direBans.value[i] = banDataDire;
    } else {
      DIRE_BANS.value[i] = "black_image";
      direState.value = phase;
      direBans.value[i] = "none";
    }

    if (banDataRadiant) {
      RADIANT_BANS.value[i] = banDataRadiant;
      radiantState.value = banDataRadiant;
      radiantBans.value[i] = banDataRadiant;
    } else {
      RADIANT_BANS.value[i] = "black_image";
      radiantState.value = phase;
      radiantBans.value[i] = "none";
    }
  }

  if (activeTeam.value === "radiant" && phase === "banning") {
    const nextRadiantBanIndex = radiantBans.value.findIndex(
      (ban: any) => ban === "none"
    );
    if (nextRadiantBanIndex !== -1) {
      radiantBans.value[nextRadiantBanIndex] = "banning";
    }
  }

  if (activeTeam.value === "dire" && phase === "banning") {
    const nextDireBanIndex = direBans.value.findIndex((ban: any) => ban === "none");
    if (nextDireBanIndex !== -1) {
      direBans.value[nextDireBanIndex] = "banning";
    }
  }

  for (let i = 0; i <= 4; i++) {
    const pickKeyDire = `pick${i}_class`;
    const pickDataDire = data.draft.team3[pickKeyDire];
    const pickKeyRadiant = `pick${i}_class`;
    const pickDataRadiant = data.draft.team2[pickKeyRadiant];

    if (pickDataDire) {
      DIRE_PICKS.value[i] = pickDataDire;
      direState.value = pickDataDire;
      direPicks.value[i] = pickDataDire;
    } else {
      DIRE_PICKS.value[i] = "dota2_logo_animated";
      direState.value = phase;
      direPicks.value[i] = "none";
    }

    if (pickDataRadiant) {
      RADIANT_PICKS.value[i] = pickDataRadiant;
      radiantState.value = pickDataRadiant;
      radiantPicks.value[i] = pickDataRadiant;
    } else {
      RADIANT_PICKS.value[i] = "dota2_logo_animated";
      radiantState.value = phase;
      radiantPicks.value[i] = "none";
    }
  }

  if (activeTeam.value === "radiant" && phase === "picking") {
    const nextRadiantPickIndex = radiantPicks.value.findIndex(
      (pick: any) => pick === "none"
    );
    if (nextRadiantPickIndex !== -1) {
      radiantPicks.value[nextRadiantPickIndex] = "picking";
    }
  }

  if (activeTeam.value === "dire" && phase === "picking") {
    const nextDirePickIndex = direPicks.value.findIndex((pick: any) => pick === "none");
    if (nextDirePickIndex !== -1) {
      direPicks.value[nextDirePickIndex] = "picking";
    }
  }

  console.table({
    "Draft Active Time Remaining": formatTime(DRAFT_ACTIVE_TIME_REMAINING),
    "Radiant Bonus Time": formatTime(RADIANT_BONUS_TIME),
    "Dire Bonus Time": formatTime(DIRE_BONUS_TIME),
    "Active Team": activeTeam.value,
    "Current Phase": phase,
    "Radiant State": radiantState.value,
    "Dire State": direState.value,
    "Radiant Picks": radiantPicks.value,
    "Dire Picks": direPicks.value,
    "Radiant Bans": radiantBans.value,
    "Dire Bans": direBans.value,
  });
});

</script>

<template>


  <div class="relative h-screen w-screen">
    <div class="absolute bottom-0 w-full h-[500px] flex items-end">
      <div class="flex flex-row w-full px-3 pb-3">
        <div class="flex flex-row w-full">
          <div class="flex flex-col w-full">
            <div class="flex flex-row">

              <div v-for="(pick, index) in radiantPicks" :key="index">
                <HeroPick :hero_name="pick" />
              </div>

            </div>
            <div class="flex flex-row bg-red-600">

              <div v-for="(ban, index) in radiantBans" :key="index">
                <Heroban :hero_name="ban" />
              </div>

            </div>
          </div>

          <CenterScreen :turn="activeTeam" :activeteam_time_remaining="formatTime(DRAFT_ACTIVE_TIME_REMAINING)"
            :radiant_bonus_time="formatTime(RADIANT_BONUS_TIME)" :dire_bonus_time="formatTime(DIRE_BONUS_TIME)" />

          <div class="flex flex-col w-full">
            <div class="flex flex-row-reverse">

              <div v-for="(pick, index) in direPicks" :key="index">
                <HeroPick :hero_name="pick" />
              </div>

            </div>
            <div class="flex flex-row-reverse bg-red-600">
              <div v-for="(ban, index) in direBans" :key="index">
                <Heroban :hero_name="ban" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>



</template>
<style scoped></style>