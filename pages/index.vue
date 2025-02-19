<template>
    <main>
        <div class="flex min-h-svh">
            <div class="min-w-80 bg-blue-300">
                <div>
                    <ul class="py-5">
                        <li v-for="(item, index) in menuItems" :key="index" class="px-2 cursor-pointer" @click="handleMenuClick(item)">
                            {{item.label}}
                        </li>
                    </ul>
                </div>
            </div>
            <div class="flex-1 flex items-center justify-center bg-green-300">
                <ScreenOne v-if="currentScreen === 'screen_1'" />
                <ScreenTwo v-else-if="currentScreen === 'screen_2'" />
                <ScreenThree v-else-if="currentScreen === 'screen_3'" />
            </div>
        </div>
    </main>
</template>
<script setup lang="ts">
import type { ScreenItem, ScreenType } from '~/types/dashboard';


const menuItems = ref<ScreenItem[]>([
    {
        label: "Screen 1",
        value: "screen_1"
    },
    {
        label: "Screen 2",
        value: "screen_2"
    },
    {
        label: "Screen 3",
        value: "screen_3"
    }
]);
const currentScreen = ref<ScreenType>("screen_1");

const handleMenuClick = (screenItem: ScreenItem) => {
    currentScreen.value = screenItem.value as ScreenType;
}
</script>