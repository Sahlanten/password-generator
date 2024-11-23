<script setup>
import Checkbox from "./components/checkbox.vue";
import MainButton from "./components/MainButton.vue";
import Header from "./components/Header.vue";
import Range from "./components/Range.vue";
import { ref } from "vue";

let generatedPassword = ref("123456789");
let passwordLength = ref(9);

// Array of options for including specific character types in the password
let characterOptions = ref([
  {
    label: "Include Lowercase Letters", // Description for UI
    isSelected: false, // Indicates if lowercase letters should be included
    optionKey: "lowercase", // Unique identifier for this option
  },
  {
    label: "Include Uppercase Letters",
    isSelected: false,
    optionKey: "uppercase",
  },
  {
    label: "Include Numbers",
    isSelected: true, // Default selection
    optionKey: "numbers",
  },
  {
    label: "Include Symbols",
    isSelected: false,
    optionKey: "symbols",
  },
]);

// Toggles the selected state of a character option based on its index
function toggleCharacterOption(index) {
  characterOptions.value[index].isSelected = !characterOptions.value[index].isSelected;
}git remote add origin https://github.com/Sahlanten/password-generator.git

// Updates the password length based on user input
function updatePasswordLength(newLength) {
  passwordLength.value = newLength;
}

// Generates the password based on the selected options and length
function generatePassword() {
  const pool = [];

  if (characterOptions.value.find((option) => option.optionKey === "lowercase")?.isSelected) {
    pool.push(...getRandomCharacters("abcdefghijklmnopqrstuvwxyz", passwordLength.value));
  }
  if (characterOptions.value.find((option) => option.optionKey === "uppercase")?.isSelected) {
    pool.push(...getRandomCharacters("ABCDEFGHIJKLMNOPQRSTUVWXYZ", passwordLength.value));
  }
  if (characterOptions.value.find((option) => option.optionKey === "numbers")?.isSelected) {
    pool.push(...getRandomCharacters("0123456789", passwordLength.value));
  }
  if (characterOptions.value.find((option) => option.optionKey === "symbols")?.isSelected) {
    pool.push(...getRandomCharacters("!@#$%^&*()_+{}|:\"<>?-=[];',./~`", passwordLength.value));
  }

  if (pool.length === 0) {
    generatedPassword.value = "Please select at least one option";
    return;
  }

  // Shuffle and select characters
  const shuffled = pool.sort(() => 0.5 - Math.random());
  generatedPassword.value = shuffled.slice(0, passwordLength.value).join("");
}

// Helper to get random characters from a string
function getRandomCharacters(source, count) {
  return Array.from({ length: count }, () => source[Math.floor(Math.random() * source.length)]);
}
</script>

<template>
  <div class="container">
    <Header>{{ generatedPassword }}</Header>

    <Range @length-data="updatePasswordLength"></Range>

    <div class="settings">
      <Checkbox
        v-for="(option, index) in characterOptions"
        :key="option.optionKey"
        :idName="option.optionKey"
        :labelName="option.label"
        :state="option.isSelected"
        :changeStateFunction="() => toggleCharacterOption(index)"
      />
    </div>

    <MainButton @click="generatePassword">Generate Password</MainButton>
  </div>
</template>

<style lang="scss" scoped>
button {
  border: 0;
  outline: 0;
}

.container {
  margin: 40px 0;
  width: 400px;
  height: 600px;
  padding: 10px 25px;
  background: #0a0e31;
  border-radius: 10px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.45), 0 4px 8px rgba(0, 0, 0, 0.35),
    0 8px 12px rgba(0, 0, 0, 0.15);
  font-family: "Montserrat";
  h2.title {
    font-size: 1.75rem;
    margin: 10px -5px;
    margin-bottom: 30px;
    color: #fff;
  }
}

$field-color: rgba(255, 255, 255, 0.08);
$field-height: 65px;
$field-border-radius: 8px;

.result {
  position: relative;
  width: 100%;
  height: 65px;
  overflow: hidden;
  &__info {
    &.right {
      right: 8px;
    }
    position: absolute;
    bottom: 4px;
    color: #fff;
    font-size: 0.8rem;
    opacity: 0;
  }
  &__viewbox {
    width: 100%;
    height: 100%;
    background: $field-color;
    border-radius: $field-border-radius;
    color: #fff;
    isstate-align: center;
    line-height: 65px;
  }
  #copy-btn {
    position: absolute;
    top: var(--y);
    left: var(--x);
    width: 38px;
    height: 38px;
    background: #fff;
    border-radius: 50%;
    opacity: 0;
    transform: translate(-50%, -50%) scale(0);
    transition: all 350ms cubic-bezier(0.175, 0.885, 0.32, 1.275);
    cursor: pointer;
    z-index: 2;
    &:active {
      box-shadow: 0 0 0 200px rgba(255, 255, 255, 0.08);
    }
  }
  &:hover {
    #copy-btn {
      opacity: 1;
      transform: translate(-50%, -50%) scale(1.35);
    }
  }
}
.field-title {
  position: absolute;
  top: -10px;
  left: 8px;
  transform: translateY(-50%);
  font-weight: 800;
  color: rgba(255, 255, 255, 0.5);
  text-transform: uppercase;
  font-size: 0.65rem;
  pointer-events: none;
  user-select: none;
}

.options {
  width: 100%;
  height: auto;
  margin: 50px 0;
}
.range__slider {
  position: relative;
  width: 100%;
  height: calc(#{$field-height} - 10px);
  display: flex;
  justify-content: center;
  align-items: center;
  background: $field-color;
  border-radius: $field-border-radius;
  margin: 30px 0;

  &::before,
  &::after {
    position: absolute;
    color: #fff;
    font-size: 0.9rem;
    font-weight: bold;
  }
  &::before {
    content: attr(data-min);
    left: 10px;
  }
  &::after {
    content: attr(data-max);
    right: 10px;
  }
  .length__title::after {
    content: attr(data-length);
    position: absolute;
    right: -16px;
    font-variant-numeric: tabular-nums;
    color: #fff;
  }
}

$range-handle-color: rgb(255, 255, 255) !default;
$range-handle-color-hover: rgb(212, 212, 212) !default;
$range-handle-size: 20px !default;

$range-track-color: rgba(255, 255, 255, 0.314) !default;
$range-track-height: 2px !default;

$range-label-width: 60px !default;

#slider {
  -webkit-appearance: none;
  width: calc(100% - (#{$range-label-width + 10px}));
  height: $range-track-height;
  border-radius: 5px;
  background: $range-track-color;
  outline: none;
  padding: 0;
  margin: 0;
  cursor: pointer;

  // Range Handle
  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: $range-handle-size;
    height: $range-handle-size;
    border-radius: 50%;
    background: $range-handle-color;
    cursor: pointer;
    transition: all 0.15s ease-in-out;
    &:hover {
      background: $range-handle-color-hover;
      transform: scale(1.2);
    }
  }
  &::-moz-range-thumb {
    width: $range-handle-size;
    height: $range-handle-size;
    border: 0;
    border-radius: 50%;
    background: $range-handle-color;
    cursor: pointer;
    transition: background 0.15s ease-in-out;
    &:hover {
      background: $range-handle-color-hover;
    }
  }
}

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

.support {
  position: fixed;
  right: 10px;
  bottom: 10px;
  padding: 10px;
  display: flex;
}
a {
  margin: 0 20px;
  color: #fff;
  font-size: 2rem;
  transition: all 400ms ease;
}

a:hover {
  color: #222;
}

.github-corner svg {
  position: absolute;
  right: 0;
  top: 0;
  mix-blend-mode: darken;
  color: #eeeeee;
  fill: #353535;
  clip-path: polygon(0 0, 100% 0, 100% 100%);
}
.github-corner:hover .octo-arm {
  animation: octocat-wave 0.56s;
}
@keyframes octocat-wave {
  0%,
  100% {
    transform: rotate(0);
  }
  20%,
  60% {
    transform: rotate(-20deg);
  }
  40%,
  80% {
    transform: rotate(10deg);
  }
}
</style>
