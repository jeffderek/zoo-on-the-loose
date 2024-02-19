<template>
    <div
        id="spinner-container"
        :style="cssProps">
        <div class="spinner"></div>
    </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
    size: {
        type: Number,
        default: 80
    }
});

let cssProps = computed(() => {
    return {
        "--loading-spinner-size": props.size + "px"
    };
});
</script>

<style scoped lang="scss">
#spinner-container {
    height: 100%;
    width: 100%;
    display: flex;
    align-items: center;

    justify-content: center;

    @keyframes spinner {
        0% {
            transform: translate3d(-50%, -50%, 0) rotate(0deg);
        }
        100% {
            transform: translate3d(-50%, -50%, 0) rotate(360deg);
        }
    }

    .spinner {
        opacity: 1;
        position: relative;
        transition: opacity linear 0.1s;

        &::before {
            animation: 2s linear infinite spinner;
            border: solid 3px black;
            border-bottom-color: white;
            border-radius: 50%;
            content: "";
            height: var(--loading-spinner-size);
            width: var(--loading-spinner-size);
            left: 50%;
            position: absolute;
            top: 50%;
            transform: translate3d(-50%, -50%, 0);
            transform-origin: center;
            will-change: transform;
        }
    }
}
</style>
