<template>
    <main>
        <div class="flex min-h-svh">
            <div class="min-w-80 bg-blue-300">
                <div>
                    <ul class="py-5">
                        <li v-for="(item, index) in menuItems" :key="index" class="px-2 cursor-pointer"
                            @click="handleMenuClick(item)">
                            {{ item.name }}
                        </li>
                    </ul>
                </div>
            </div>
            <div class="flex-1 flex items-center justify-center bg-green-300">
                <ScreenOne v-if="currentScreen === 'screen_1'" :menuItems="menuItems" />
                <ScreenTwo v-else-if="currentScreen === 'screen_2'" />
                <ScreenThree v-else-if="currentScreen === 'screen_3'" />
            </div>
        </div>
    </main>
</template>
<script setup lang="ts">
import { useApi } from "~/composables/useAPI";
import type { ScreenItem, ScreenType } from "~/types/dashboard";

const { api } = useApi();

const menuItems = ref<ScreenItem[]>();
const currentScreen = ref<ScreenType>("screen_1");

const handleMenuClick = (screenItem: ScreenItem) => {
    currentScreen.value = screenItem.key as ScreenType;
};

const getScreens = async () => {
    try {
        const response = await api.get("/api/screens");
        menuItems.value = response.data;
    } catch (error) {
        console.error(error);
    }
};

onMounted(() => {
    getScreens();
})
</script>
