<template>
  <div class="global-navigation-bar-container">
    <div class="navigate-button-wrapper" v-on:click="methods.moveTo('/')"
         :class="{ selected: navigateMenuStore.state.activeHomeMenu }">
      <button class="navigate-button">
        <div class="button-content">
          <FontAwesomeIcon :icon="[navigateMenuStore.state.activeHomeMenu ? 'fas' : 'far', 'clock']" class="fa-xl" />
          <span class="menu-name">예정</span>
        </div>
      </button>
    </div>
    <div class="navigate-button-wrapper" v-on:click="methods.moveTo('/calendar')"
         :class="{ selected: navigateMenuStore.state.activeCalendarMenu }">
      <button class="navigate-button">
        <div class="button-content">
          <FontAwesomeIcon :icon="['fas', 'calendar-days']" class="fa-xl" />
          <span class="menu-name">일정</span>
        </div>
      </button>
    </div>
    <div class="navigate-button-wrapper" v-on:click="methods.moveTo('/discuss')"
         :class="{ selected: navigateMenuStore.state.activeDiscussMenu }">
      <button class="navigate-button">
        <div class="button-content">
          <FontAwesomeIcon :icon="[navigateMenuStore.state.activeDiscussMenu ? 'fas' : 'far', 'comments']"
                           class="fa-xl" />
          <span class="menu-name">토론</span>
        </div>
      </button>
    </div>
    <div class="navigate-button-wrapper" v-on:click="methods.moveTo('/families')"
         :class="{ selected: navigateMenuStore.state.activeFamiliesMenu }">
      <button class="navigate-button">
        <div class="button-content">
          <FontAwesomeIcon :icon="['fas', 'users']" class="fa-xl" />
          <span class="menu-name">패밀리</span>
        </div>
      </button>
    </div>
    <div class="navigate-button-wrapper" v-on:click="methods.moveTo('/settings')"
         :class="{ selected: navigateMenuStore.state.activeSettingsMenu }">
      <button class="navigate-button">
        <div class="button-content">
          <FontAwesomeIcon :icon="['fas', 'gear']" class="fa-xl" />
          <span class="menu-name">설정</span>
        </div>
      </button>
    </div>

  </div>
</template>
<script setup lang="ts">

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { useNavigateMenuStore } from '@/stores/NavigateMenuStore'
import { useRouter } from 'vue-router'

const navigateMenuStore = useNavigateMenuStore();
const router = useRouter()
const methods = {
  moveTo(path: string) {
    router.push(path);
  }
}
</script>
<style scoped lang="scss">
@import "@assets/main";

.global-navigation-bar-container {
  flex-shrink: 0;
  height: $footer-height;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  padding: 5px 8px 15px 8px;
  background-color: white;

  .navigate-button-wrapper {
    border-radius: 5px;
    color: $standard-weight-gray-in-white;
    width: 100%;
    display: flex;
    user-select: none;
    justify-content: center;
    align-items: center;

    .navigate-button {

      .button-content {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        .menu-name {
          font-weight: bold;
          font-size: .64rem;
        }
      }
    }

    &.selected {
      color: $text-signature-purple !important;
      animation: bounce-in .3s;
    }
  }
}

@media (prefers-color-scheme: dark) {
  .global-navigation-bar-container {
    background-color: $dark-mode-background;

    .navigate-button-wrapper {
      color: $standard-gray-in-white;

      .navigate-button {

        .button-content {

          .menu-name {

          }
        }
      }

      &.selected {
        color: $light-signature-purple !important;
      }
    }
  }
}
</style>
