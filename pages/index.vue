<template>
    <section>
        <div class="loading-container" v-if="isloading">
            <Loading class="loader" />
        </div>
        <div v-else>
            <CarForm />
            <GalleryCard />
        </div>
    </section>
</template>
<script setup>
import { onMounted, defineAsyncComponent } from "vue";
import { storeToRefs } from "pinia";
import useGlobalStore from "../stores/globalStore";

useHead({
    titleTemplate: "Home|CarNation",
});
definePageMeta({
    middleware: ["auth"],
});

const store = useGlobalStore();
const { getIsLoading: isloading } = storeToRefs(store);
const { getCarDetails: getCarAPI } = store;

onMounted(async () => {
    await getCarAPI();
});

// Create an async component using defineAsyncComponent
const CarForm = defineAsyncComponent(() => import("../components/CarForm.vue"));
const GalleryCard = defineAsyncComponent(() =>
    import("../components/GalleryCard.vue")
);
</script>

<style scoped>
.loading-container {
    position: relative;
    height: 30vh;
}
.loader {
    position: absolute;
    top: 90%;
    left: 50%;
    transform: translate(-50%, -50%);
}
</style>
