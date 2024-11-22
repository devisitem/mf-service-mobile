<template>
  <div class="create-category-container">
    <div class="create-input-form">
      <BlinkInput ref="nameInput" id="category-name" type="text" label="카테고리명"
                  :placeHolder="defaultValues.name"
                  :is-hold="state.inputHold" :validate="methods.validateName"
                  warning-message="카테고리 이름을 입력해 주세요. (최대 50자)" :no-mark="true"
      />
      <BlinkInput ref="descriptionInput" id="category-description" type="text" label="설명"
                  :placeHolder="defaultValues.description"
                  :is-hold="state.inputHold" :validate="methods.validateDescription"
                  warning-message="설명은 최대 100자까지 가능합니다." :no-mark="true"
      />
      <div class="for-example-color">
        <span class="explain-text">적용 예시:</span>
        <span class="sample-schedule" :style="{
          backgroundColor: `#${state.selectedColor}`,
          color: ColorUtil.isDarkColor(state.selectedColor) ? 'white' : 'black'
        }">🚨10시 유산소 운동🚨</span>
      </div>
      <ColorSelector :select-color="methods.whenSelectedColor" />
    </div>
    <div class="control-panel">
      <div class="control-button" v-on:click="methods.createCategory">
        <span class="button-text">생성</span>
      </div>
      <div class="control-button" v-on:click="() => navigateStackStore.pullComponent()">
        <span class="button-text">취소</span>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import BlinkInput from '@/components/global/BlinkInput.vue'
import { reactive, ref } from 'vue'
import { useNavigateStackStore } from '@/stores/NavigateStackStore'
import type InputComponent from '@/classes/InputComponent'
import { ex } from '@/utils/Undefinable'
import ColorSelector from '@/views/global/ColorSelector.vue'
import { useAlertStore } from '@/stores/AlertStore'
import { call } from '@/utils/NetworkUtil'
import { RequestBody } from '@/classes/api-spec/schedule/CreateScheduleCategory'
import Schedule from '@/constant/api-meta/Schedule'
import { useScheduleCategoryStore } from '@/stores/ScheduleCategoryStore'
import ColorUtil from '@/utils/ColorUtil'


const scheduleCategoryStore = useScheduleCategoryStore()
const alertStore = useAlertStore()
const navigateStackStore = useNavigateStackStore()
const defaultValues = {
  name: '친구 약속',
  description: '대학교 동기'
}
const nameInput = ref<InputComponent | null>(null)
const descriptionInput = ref<InputComponent | null>(null)
const inputValues = reactive({
  name: '',
  description: ''
})
const state = reactive({
  inputHold: false,
  isSubmittable: false,

  isValidName: false,
  isValidDescription: false,
  selectedColor: ''
})

const methods = {
  validateName() {
    inputValues.name = ex(nameInput.value)
      .to<InputComponent, string>(value => value.input.value, '')
    state.isValidName = 0 < inputValues.name.length && inputValues.name.length <= 50
    if (!state.isValidName) {
      nameInput.value?.input.focus()
    }
    return state.isValidName
  },
  validateDescription() {
    inputValues.description = ex(descriptionInput.value)
      .to<InputComponent, string>(value => value.input.value, '')
    state.isValidDescription = inputValues.description.length <= 100
    if (!state.isValidDescription) {
      descriptionInput.value?.input.focus()
    }
    return state.isValidDescription
  },
  whenSelectedColor(color: string) {
    state.selectedColor = color
  },
  checkAllInput() {
    state.isSubmittable = this.validateName() && this.validateDescription() && state.selectedColor !== ''
  },
  createCategory() {
    this.checkAllInput()

    if (!state.isSubmittable) {
      alertStore.info('생성 실패', '입력값을 확인해 주세요.')
      return
    }
    const requestBody = new RequestBody(inputValues.name, state.selectedColor, inputValues.description)
    call<RequestBody, any>(Schedule.CreateCategory, requestBody, (response) => {
      alertStore.success('카테고리 생성', `"${inputValues.name}" 카테고리를 생성했어요!`)
      navigateStackStore.pullComponent()
      scheduleCategoryStore.fetchScheduleCategories()
    })
  }
}
</script>
<style scoped lang="scss">
@import '@assets/main';

.create-category-container {

  .create-input-form {

    .for-example-color {
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 50px;

      .explain-text {
        padding: 0 10px;
      }

      .sample-schedule {
        border-radius: 5px;
      }
    }
  }

  padding: 0 20px;

  .control-panel {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;

    .control-button {
      transition: $duration;
      border-radius: 5px;
      padding: 5px 10px;
      user-select: none;

      .button-text {
        padding: 0;
      }

    }
  }
}
</style>
