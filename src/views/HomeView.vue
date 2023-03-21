<template>
    <main>
        <button id="big-red-button" @click="randomize()">Click Me!</button>
        <template v-if="randomized">
            <div id="labels">
                <div class="label">
                    <h1>Place the {{ animal.name }}</h1>
                </div>
                <div class="label">
                    <h1>{{ action.name }}</h1>
                </div>
            </div>
            <div id="images">
                <div class="img">
                    <img :src="getImgUrl(animal.icon)" />
                </div>
                <div class="img">
                    <img :src="getImgUrl(action.icon)" />
                </div>
            </div>
        </template>
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

    var elem = document.documentElement;
    if (elem.requestFullscreen) {
        elem.requestFullscreen();
    } else if (elem.webkitRequestFullscreen) {
        /* Safari */
        elem.webkitRequestFullscreen();
    } else if (elem.msRequestFullscreen) {
        /* IE11 */
        elem.msRequestFullscreen();
    }
}
</script>

<style scoped lang="scss">
main {
    height: 100%;
    // border: 4px solid green;

    // display: grid;
    // grid-template-columns: repeat(2, 1fr);
    // grid-auto-rows: 1fr;
    // gap: 1rem;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;

    #big-red-button {
        width: 100%;
        // height: 100px;
        background-color: $ss-color-dan;
        color: $ss-color-white;
        font-size: 2.5rem;

        // grid-column: 1 / 3;
        flex: 0 0 100px;
    }

    #labels {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        width: 100%;

        .label {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-end;

            height: 100px;
            h1 {
                text-align: center;
                padding: 1rem;
            }
        }
    }

    #images {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        width: 100%;
        flex-grow: 0;

        overflow: hidden;

        .img {
            padding: 1rem;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100%;
            overflow: hidden;

            img {
                width: 100%;
                height: 100%;
                // width: auto;
                // height: auto;
                object-fit: contain;
            }
        }
    }
}
</style>
