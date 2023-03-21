<template>
    <main>
        <button id="big-red-button" @click="randomize()">Click Me!</button>
        <div id="result" v-if="randomized">
            <div class="result-section">
                <h2>Place the {{ animal.name }}</h2>
                <img :src="getImgUrl(animal.icon)" />
            </div>
            <div class="result-section">
                <h2>{{ action.name }}</h2>
                <img :src="getImgUrl(action.icon)" />
            </div>
        </div>
    </main>
</template>

<script setup>
import { ref, onMounted } from 'vue';

function getImgUrl(fileName) {
    const imgUrl = new URL(`/src/assets/icons/${fileName}.png`, import.meta.url).href;
    return imgUrl;
}

console.log(getImgUrl('arctic'));

const randomized = ref(false);
const animal = ref(null);
const action = ref(null);
let lastAnimalRnd = 9999;
let lastActionRnd = 9999;

let animals = [
    {
        name: 'Zebra',
        icon: 'zebra',
    },
    {
        name: 'Camel',
        icon: 'camel',
    },
    {
        name: 'Monkey',
        icon: 'monkey',
    },
    {
        name: 'Whale',
        icon: 'whale',
    },
    {
        name: 'Polar Bear',
        icon: 'polar-bear',
    },
];

let actions = [
    {
        name: 'Near the Zebra',
        icon: 'zebra',
    },
    {
        name: 'Near the Camel',
        icon: 'camel',
    },
    {
        name: 'Near the Monkey',
        icon: 'monkey',
    },
    {
        name: 'Near the Whale',
        icon: 'whale',
    },
    {
        name: 'Near the Polar Bear',
        icon: 'polar-bear',
    },
    {
        name: 'In the Arctic',
        icon: 'arctic',
    },
    {
        name: 'In the Desert',
        icon: 'desert',
    },
    {
        name: 'In the Jungle',
        icon: 'jungle',
    },
    {
        name: 'In the Ocean',
        icon: 'ocean',
    },
    {
        name: 'In the Savannah',
        icon: 'savannah',
    },
    {
        name: 'Under a Blanket',
        icon: 'blanket',
    },
    {
        name: 'Under a Chair',
        icon: 'chair',
    },
    {
        name: 'On a Pillow',
        icon: 'pillow',
    },
    {
        name: 'On a couch',
        icon: 'sofa',
    },
    {
        name: 'By a Window',
        icon: 'window',
    },
];

function randomize() {
    let animalRnd = lastAnimalRnd;

    while (animalRnd == lastAnimalRnd) {
        animalRnd = Math.floor(Math.random() * animals.length);
    }
    lastAnimalRnd = animalRnd;
    animal.value = animals[animalRnd];

    let actionRnd = lastActionRnd;
    while (actionRnd == lastActionRnd || actionRnd == animalRnd) {
        actionRnd = Math.floor(Math.random() * actions.length);
    }
    lastActionRnd = actionRnd;
    action.value = actions[actionRnd];

    randomized.value = true;
}
</script>

<style scoped lang="scss">
main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    gap: 1rem;

    padding: 1rem;

    #big-red-button {
        width: 90%;
        height: 15%;
        background-color: $ss-color-dan;
        color: $ss-color-white;
        font-size: 2.5rem;
    }

    #result {
        display: grid;
        width: 90%;
        grid-template-columns: repeat(2, 1fr);
        gap: 1rem;

        .result-section {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
        }
    }
}
</style>
