<script setup>
let emit = defineEmits(["is-checked"]);

let props = defineProps({
  labelName: String,
  idName: String,
  state: Boolean,
  changeStateFunction: Function,
});
</script>

<template>
  <div class="setting">
    <input type="checkbox" :id="idName" :checked="state" @click="changeStateFunction" />
    <label :for="idName">{{ labelName }} {{ state }}</label>
  </div>
</template>

<style lang="scss" scoped>
$field-color: rgba(255, 255, 255, 0.08);
$field-height: 65px;
$field-border-radius: 8px;

.settings {
  position: relative;
  height: auto;
  widows: 100%;
  display: flex;
  flex-direction: column;

  .setting {
    position: relative;
    width: 100%;
    height: calc(#{$field-height} - 10px);
    background: $field-color;
    border-radius: $field-border-radius;
    display: flex;
    align-items: center;
    padding: 10px 25px;
    color: #fff;
    margin-bottom: 8px;
    input {
      opacity: 0;
      position: absolute;
      + label {
        user-select: none;
        &::before,
        &::after {
          content: "";
          position: absolute;
          transition: 150ms cubic-bezier(0.24, 0, 0.5, 1);
          transform: translateY(-50%);
          top: 50%;
          right: 10px;
          cursor: pointer;
        }
        &::before {
          height: 30px;
          width: 50px;
          border-radius: 30px;
          background: rgba(214, 214, 214, 0.434);
        }
        &::after {
          height: 24px;
          width: 24px;
          border-radius: 60px;
          right: 32px;
          background: #fff;
        }
      }
      &:checked {
        & + label:before {
          background: #5d68e2;
          transition: all 150ms cubic-bezier(0, 0, 0, 0.1);
        }
        & + label:after {
          right: 14px;
        }
      }
      &:focus {
        + label:before {
          box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.75);
        }
      }
      &:disabled {
        + label {
          &:before,
          &:after {
            cursor: not-allowed;
          }
          &:before {
            background: #4f4f6a;
          }
          &:after {
            background: #909090;
          }
        }
      }
    }
  }
}
</style>
