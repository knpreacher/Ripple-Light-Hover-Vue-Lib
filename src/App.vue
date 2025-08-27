<script setup lang="ts">
import { ref } from 'vue';
import { CircleLightWrapper } from '../lib'

const wrapperProps = ref({
  size: 2,
  borderCircleSize: 300,
  innerCircleSize: 300,
  innerCircleOpacity: 0.05,
  color: '#f26645'
})
</script>

<template>
  <div class="container">
    <div style="margin-bottom: 12px;">
      <label>
        <span>Color</span>
        <input type="color" v-model="wrapperProps.color"></input>
      </label>
      <label>
        <span>Size</span>
        <input type="number" v-model="wrapperProps.size"></input>
      </label>
    </div>
    <div>{{ wrapperProps }}</div>
    <div style="margin: 32px; position: relative;">
      <circle-light-wrapper #default="{ cls, wrapper }" v-bind="wrapperProps" wrapper-extra-class="rounded">
      <div class="row">
        <div class="col" v-for="i in 40" :key="i">
          <component :is="wrapper">
            <div :class="cls" class="card rounded">
              kek {{ i }}
            </div>
          </component>
        </div>
      </div>
    </circle-light-wrapper>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100%;
}

.row {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  width: 100%;
  height: 100%;
  gap: 10px;

  .col {
    max-width: 20%;
    width: 100%;
  }
}

.rounded {
  border-radius: 8px;
}

.card {
  background-color: rgb(24, 24, 24);
  height: 100%;
}

label>span {
  display: block;
  margin-bottom: 4px;
}
</style>
