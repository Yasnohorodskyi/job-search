<script lang="ts" setup>
import { ref, computed } from 'vue';

import { useUserStore } from '@/stores/user';

import ActionButton from '@/components/Shared/ActionButton.vue';
import ProfileImage from '@/components/Navigation/ProfileImage.vue';
import TheSubnav from '@/components/Navigation/TheSubnav.vue';

const menuItems = ref([
  { text: 'Teams', url: '/teams' },
  { text: 'Locations', url: '/' },
  { text: 'Life at Bobo Corp', url: '/' },
  { text: 'How we hire', url: '/' },
  { text: 'Students', url: '/' },
  { text: 'Jobs', url: '/jobs/results' }
]);

const useStore = useUserStore();
const loginUser = useStore.LOGIN_USER;
const LOGIN_USER = computed(() => useStore.isLoggedIn);
const headerHeightClass = computed(() => {
  return {
    'h-16': !LOGIN_USER.value,
    'h-32': LOGIN_USER.value
  };
});
</script>

<template>
  <header :class="['w-full', 'text-sm', headerHeightClass]">
    <div class="fixed left-0 top-0 h-16 w-full bg-white">
      <div
        class="mx-auto flex h-full flex-nowrap border-b border-solid border-brand-gray-1 px-8"
      >
        <router-link
          :to="{ name: 'Home' }"
          class="flex h-full items-center text-xl"
          >Bobo Careers
        </router-link>

        <nav class="ml-12 h-full">
          <ul class="flex h-full list-none">
            <li
              v-for="menuItem in menuItems"
              :key="menuItem.text"
              class="ml-9 h-full first:ml-0"
            >
              <router-link
                :to="menuItem.url"
                class="flex h-full items-center py-2.5"
                >{{ menuItem.text }}</router-link
              >
            </li>
          </ul>
        </nav>

        <div class="ml-auto flex h-full items-center">
          <profile-image v-if="LOGIN_USER" />
          <action-button v-else text="Sign in" @click="loginUser" />
        </div>
      </div>

      <the-subnav v-if="LOGIN_USER" />
    </div>
  </header>
</template>
