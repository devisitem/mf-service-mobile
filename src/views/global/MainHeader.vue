<template>
  <div class="main-header-container">
    <FamilySelector v-if="memberInfoStore.getCurrentAccountRole().isGrantedFrom(AccountRole.MEMBER)" />
    <ProfilePreview/>
    <NotificationIndicator />
    <SimpleNotifier/>
  </div>
</template>
<script setup lang="ts">
import ProfilePreview from '@/components/header/ProfilePreview.vue'
import FamilySelector from '@/components/header/FamilySelector.vue'
import { AccountRole } from '@/constant/AccountRole'
import { useMemberInfoStore } from '@/stores/MemberInfoStore'
import SimpleNotifier from '@/components/global/SimpleNotifier.vue'
import NotificationIndicator from '@/components/header/NotificationIndicator.vue'

const memberInfoStore = useMemberInfoStore()
</script>

<style scoped lang="scss">
@import "@assets/main";

.main-header-container {
  padding: 5px 8px;
  border-bottom: 1px solid $standard-light-gray-in-white;
  display: flex;
  flex-shrink: 0;
}

@media (prefers-color-scheme: dark) {
  .main-header-container {
    border-bottom: 1px solid $dark-mode-border;
  }
}
</style>
