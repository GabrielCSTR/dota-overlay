<script setup lang="ts">
const propHeroPick = defineProps({
    hero_name: String
})

const getImageUrl = (hero_name: string) => {
    const image = new URL(`../assets/images/heroes_animated/npc_dota_hero_${hero_name}.webm`, import.meta.url).href;
    return image || '';
};

</script>

<template>
    <div v-if="propHeroPick.hero_name !== 'none'" class="w-full h-full bg-black">
        <video autoplay muted loop class="w-[146.74px] h-[219.57px] object-fill animate-fade-in"
            style="transition: filter 0.3s ease-in-out; z-index: 2;">
            <source :src="getImageUrl(propHeroPick?.hero_name)" type="video/webm" />
        </video>
    </div>
    <div v-else-if="propHeroPick.hero_name === 'picking'" class="w-full h-full bg-black">
        <video autoplay muted loop class="w-[146.74px] h-[219.57px] object-fill animate-fade-in"
            style="transition: filter 0.3s ease-in-out; z-index: 2;">
            <source src="../assets/images/dota2_logo_animated.mp4" type="video/webm" />
        </video>
    </div>
    <div v-else-if="propHeroPick.hero_name === 'none'" class="w-full h-full bg-black">
        <video autoplay muted loop class="w-[146.74px] h-[219.57px] object-fill animate-fade-in"
            style="transition: filter 0.3s ease-in-out; z-index: 2;">
            <source src="../assets/images/dota2_logo_animated.mp4" type="video/webm" />
        </video>
    </div>
</template>

<style scoped>
@keyframes fadeIn {
    0% {
        opacity: 0;
    }

    100% {
        opacity: 1;
    }
}

.animate-fade-in {
    animation: fadeIn 0.5s ease-in;
}
</style>