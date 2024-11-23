<script setup>
import { ref } from "vue";
let btnGenarate = ref();
</script>

<template>
  <button @click="onbutton" ref="btnGenarate" class="btn generate">Generate Password</button>
</template>

<style lang="scss" scoped>
button {
  border: 0;
  outline: 0;
}

$field-color: rgba(255, 255, 255, 0.08);
$field-height: 65px;
$field-border-radius: 8px;

.btn.generate {
  user-select: none;
  position: relative;
  width: 100%;
  height: 50px;
  margin: 10px 0;
  border-radius: $field-border-radius;
  color: #fff;
  border: none;
  background-image: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  letter-spacing: 1px;
  font-weight: bold;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 150ms ease;
  &:active {
    transform: translateY(-3%);
    box-shadow: 0 4px 8px rgba(255, 255, 255, 0.08);
  }
}
</style>
