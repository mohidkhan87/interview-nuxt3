<template>
    <nav aria-label="Page navigation example">
      <ul class="flex justify-center border border-[#e1e1e1] w-max rounded-lg">
        <li
          class="border-r border-[#e1e1e1] min-w-10 cursor-pointer"
          :class="{ 'pointer-events-none opacity-60': currentPage === 1 }"
        >
          <span
            class="p-2 w-full h-full flex justify-center items-center"
            @click="$emit('paginate', currentPage - 1)"
          >
            <IconChevronLeft />
          </span>
        </li>
        <li
          class="border-r border-[#e1e1e1] min-w-10 cursor-pointer"
          :class="{ 'bg-[#6655ff] text-white': pageNo === currentPage }"
          v-for="pageNo in makePageNumber"
          :key="pageNo"
        >
          <span
            class="p-2 w-full h-full flex justify-center items-center"
            @click="$emit('paginate', pageNo)"
          >
            {{ pageNo }}
          </span>
        </li>
        <li
          class="min-w-10 cursor-pointer"
          :class="{ 'pointer-events-none opacity-60': currentPage === lastPage }"
        >
          <span
            class="p-2 w-full h-full flex justify-center items-center"
            @click="$emit('paginate', currentPage + 1)"
          >
            <IconChevronRight />
          </span>
        </li>
      </ul>
    </nav>
  </template>
  
  <script setup lang="ts">
  const props = defineProps({
    currentPage: {
      type: Number,
      default: 1,
    },
    lastPage: {
      type: Number,
      default: 3,
    },
  });
  
  const makePageNumber = computed(() => {
    const pageNumbers: number[] = [];
    const diffBetweenPages = props.lastPage - props.currentPage;
    let page = props.currentPage;
  
    for (let i = 0; i <= diffBetweenPages; i++) {
      pageNumbers.push(page);
      page += 1;
      if (i === 3) {
        break;
      }
    }
  
    let previousPageIterationCounter = 0;
    if (props.currentPage > 1) {
      for (let i = props.currentPage - 1; i > 0; i--) {
        previousPageIterationCounter++;
        pageNumbers.push(i);
        if (previousPageIterationCounter >= 3) {
          break;
        }
      }
    }
  
    pageNumbers.sort((a, b) => a - b);
    return pageNumbers;
  });
  </script>
  