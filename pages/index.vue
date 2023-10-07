<template>
  <div class="container mx-auto px-5 py-2 lg:px-5 lg:pt-12">
    <Menu as="div" class="relative inline-block text-center">
      <div>
        <MenuButton
          class="inline-flex w-full justify-center gap-x-1.5 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50">
          Dogs List
          <ChevronDownIcon
            class="-mr-1 h-5 w-5 text-gray-400"
            aria-hidden="true" />
        </MenuButton>
      </div>

      <transition
        enter-active-class="transition ease-out duration-100"
        enter-from-class="transform opacity-0 scale-95 translate-x-1/2"
        enter-to-class="transform opacity-100 scale-100 translate-x-0"
        leave-active-class="transition ease-in duration-75"
        leave-from-class="transform opacity-100 scale-100 translate-x-0"
        leave-to-class="transform opacity-0 scale-95 translate-x-1/2">
        <MenuItems
          class="absolute right-0 left-1/2 z-10 mt-2 w-56 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none -translate-x-1/2">
          <div class="py-1">
            <MenuItem
              v-for="(menuItem, index) in menuItems"
              :key="index"
              v-slot="{ active }">
              <a
                :class="[
                  active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                  'block px-4 py-2 text-sm',
                ]"
                >{{ menuItem.label }}</a
              >
            </MenuItem>
          </div>
        </MenuItems>
      </transition>
    </Menu>

    <div class="-m-1 flex flex-wrap md:-m-2">
      <div
        class="flex w-1/4 flex-wrap"
        v-for="(imageUrl, index) in imageUrlsArray"
        :key="index">
        <div class="w-full p-1 md:p-2">
          <img
            alt="gallery"
            class="block h-full w-full rounded-lg object-cover object-center"
            :src="imageUrl"
            max-width="200"
            max-height="200" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";

import { onMounted, ref } from "vue";

onMounted(() => {
  fetchDogsList();
  fetchDogs();
});

const imageUrls = ref("");
const imageUrlsArray = ref([]);
const dogsList = ref([]);

const menuItems = ref([
  { label: "Account settings" },
  { label: "Support" },
  { label: "License" },
  { label: "Sign out" },
]);

async function fetchDogsList() {
  const response = await fetch("https://dog.ceo/api/breeds/list/all");
  const data = await response.json();
  dogsList.value = Object.keys(data.message);
  console.log("DogsList", dogsList.value);
}

async function fetchDogs() {
  const response = await fetch("https://dog.ceo/api/breed/hound/images");
  const data = await response.json();
  imageUrls.value = data.message;
  imageUrlsArray.value = data.message;
}
</script>
