<template>
  <main>
    <template v-if="showRandomAction">
      <div class="labels cols">
        <div class="label">
          <h1>Place the {{ animal.name }}</h1>
        </div>
        <div class="label">
          <h1>{{ action.name }}</h1>
        </div>
      </div>
      <div class="images cols">
        <div class="img">
          <img :src="getImgUrl(animal.icon)" />
        </div>
        <div class="img">
          <img :src="getImgUrl(action.icon)" />
        </div>
      </div>
    </template>
    <template v-if="showZookeeper">
      <div class="labels">
        <div class="label">
          <h1>Zookeeper!</h1>
          <h2>Return all animals to their positions</h2>
        </div>
      </div>
      <div class="images">
        <div class="img">
          <img :src="getImgUrl('zookeeper')" />
        </div>
      </div>
    </template>
    <div v-else class="content"></div>
    <button
      id="big-red-button"
      @pointerup="randomize()"
      :class="{ disabled: !allowBigRedButtonPress }"
    >
      {{ allowBigRedButtonPress ? 'Click Me!' : '' }}
      <LoadingSpinner v-if="!allowBigRedButtonPress" />
    </button>
  </main>
</template>

<script setup>
  import LoadingSpinner from '@/components/LoadingSpinner.vue';
  import { ref, onMounted } from 'vue';

  function getImgUrl(fileName) {
    const imgUrl = new URL(`/src/assets/icons/${fileName}.png`, import.meta.url)
      .href;
    return imgUrl;
  }

  console.log(getImgUrl('arctic'));

  const showRandomAction = ref(false);
  const showZookeeper = ref(false);
  const animal = ref(null);
  const action = ref(null);
  const allowBigRedButtonPress = ref(true);
  let lastAnimalRnd = 9999;
  let lastActionRnd = 9999;
  let actionsSinceLastZookeeper = 0;
  let zookeeperOdds = 0;

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
      name: 'Anywhere you want',
      icon: 'question',
    },
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
    {
      name: 'In a Box',
      icon: 'box',
    },
    {
      name: 'On a Table',
      icon: 'table',
    },
    // {
    //     name: 'Someplace Hot',
    //     icon: 'sun',
    // },
    // {
    //     name: 'Someplace Cold',
    //     icon: 'snowflakes',
    // },
    {
      name: 'Near a Shoe',
      icon: 'sneakers',
    },
    {
      name: 'In a Drawer',
      icon: 'drawer',
    },
    {
      name: 'By a Door',
      icon: 'door',
    },
    {
      name: 'Near a Book',
      icon: 'book',
    },
    {
      name: 'Near some Blocks',
      icon: 'blocks',
    },
    {
      name: 'Near a Ball',
      icon: 'ball',
    },
  ];

  let isMobile = false;

  if (
    navigator.userAgent.match(/Android/i) ||
    navigator.userAgent.match(/webOS/i) ||
    navigator.userAgent.match(/iPhone/i) ||
    navigator.userAgent.match(/iPad/i) ||
    navigator.userAgent.match(/iPod/i) ||
    navigator.userAgent.match(/BlackBerry/i) ||
    navigator.userAgent.match(/Windows Phone/i)
  ) {
    isMobile = true;
  }

  let documentElement = document.documentElement;

  function randomize() {
    if (!allowBigRedButtonPress.value) {
      return;
    }

    if (actionsSinceLastZookeeper >= 3) {
      zookeeperOdds = zookeeperOdds + Math.random() / 2;
    }

    if (zookeeperOdds >= 1) {
      console.log(`zookeeper actions ${actionsSinceLastZookeeper}`);
      actionsSinceLastZookeeper = 0;
      zookeeperOdds = 0;

      showRandomAction.value = false;
      showZookeeper.value = true;

      allowBigRedButtonPress.value = false;
      setTimeout(() => {
        allowBigRedButtonPress.value = true;
      }, 3000);
      return;
    }

    let animalRnd = lastAnimalRnd;

    while (animalRnd == lastAnimalRnd) {
      animalRnd = Math.floor(Math.random() * animals.length);
    }
    lastAnimalRnd = animalRnd;

    const nextAnimal = animals[animalRnd];

    let actionRnd = lastActionRnd;
    let nextAction;
    do {
      while (actionRnd == lastActionRnd || actionRnd == animalRnd) {
        actionRnd = Math.floor(Math.random() * actions.length);
      }
      lastActionRnd = actionRnd;
      nextAction = actions[actionRnd];
    } while (nextAction.icon == nextAnimal.icon);

    showZookeeper.value = false;
    showRandomAction.value = true;

    animal.value = nextAnimal;
    action.value = nextAction;

    actionsSinceLastZookeeper++;

    if (isMobile) {
      if (documentElement.requestFullscreen) {
        documentElement.requestFullscreen();
      } else if (documentElement.webkitRequestFullscreen) {
        /* Safari */
        documentElement.webkitRequestFullscreen();
      } else if (documentElement.msRequestFullscreen) {
        /* IE11 */
        documentElement.msRequestFullscreen();
      }
    }

    allowBigRedButtonPress.value = false;
      setTimeout(() => {
        allowBigRedButtonPress.value = true;
      }, 3000);
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

    .content {
      flex: 1;
      padding-bottom: 30px;
    }

    #big-red-button {
      width: 100%;
      // height: 100px;
      background-color: $color-red-30;
      color: $color-white;
      font-size: 3rem;

      // grid-column: 1 / 3;
      flex: 0 0 150px;

      &:active:not(.disabled) {
        background-color: $color-red-40;
      }

      &.disabled {
        opacity: 0.6;
        cursor: default;
      }
    }

    .labels {
      display: grid;
      width: 100%;

      &.cols {
        grid-template-columns: repeat(2, 1fr);
      }

      .label {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: flex-end;

        // height: 100px;
        padding: 1rem;
        h1 {
          text-align: center;
        }
      }
    }

    .images {
      display: grid;
      width: 100%;
      flex-grow: 0;

      overflow: hidden;

      &.cols {
        grid-template-columns: repeat(2, 1fr);
      }

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
