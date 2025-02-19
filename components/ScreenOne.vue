<template>
    <div class="w-2/3 mx-auto p-4 bg-white shadow-lg rounded-lg">
        <!-- Search Bar -->
        <div class="mb-4">
            <input type="text" v-model="searchQuery" placeholder="Search..."
                class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                @input="fetchResults" @keydown="handleKeyDown" />
        </div>

        <!-- Table -->
        <div class="overflow-x-auto">
            <table class="w-full border-collapse border border-gray-300">
                <thead>
                    <tr class="bg-gray-100">
                        <th class="border border-gray-300 p-2">ID</th>
                        <th class="border border-gray-300 p-2">Name</th>
                        <th class="border border-gray-300 p-2">Status</th>
                        <th class="border border-gray-300 p-2">Notes</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in results" :key="index">
                        <td class="border border-gray-300 p-2">{{ item.id }}</td>
                        <td class="border border-gray-300 p-2">{{ item.name }}</td>
                        <td class="border border-gray-300 p-2">{{ item.name }}</td>
                        <td class="border border-gray-300 p-2">{{ item.name }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { debounce } from "lodash";
import type { ScreenItem } from "~/types/dashboard";

const props = defineProps({
    menuItems: {
        type: Array as PropType<ScreenItem[]>,
        default: () => [],
    },
});

const { api } = useApi();

const searchQuery = ref("");
const results = ref<ScreenItem[]>([]);

const fetchResults = debounce(async () => {
    if (searchQuery.value) {
        results.value = [];
        try {
            const response = await api.post("/api/search_screens", {
                search: searchQuery.value.trim()
            })
            results.value = response.data.data
        } catch (error) {

        }
    } else {
        results.value = props.menuItems;
    }
}, 500);

const handleKeyDown = (event) => {
    if (event.key === "Enter") {
        fetchResults();
    }
};

watch(
    () => props.menuItems,
    (newVal) => {
        results.value = newVal;
    },
    { deep: true }
);
</script>
