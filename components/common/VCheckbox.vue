<template>
  <div>
    <input
      type="checkbox"
      v-model="checked"
      :value="value"
      class="v-checkbox"
      :id="value">
    <label
      :for="value"
    >
      {{ value }}
    </label>
  </div>
  
</template>

<script setup>
  import { watch, ref } from 'vue';
  const props = defineProps({
    modelValue: Array,
    value: String
  });
  
  const checked = ref(props.modelValue.includes(props.value));
  
  watch(checked, (v) => {
    if(!v && props.modelValue.includes(props.value)) {
      const toRemove = props.modelValue.indexOf(props.value);
      props.modelValue.splice(toRemove, 1);
    } else if(v && !props.modelValue.includes(props.value)) {
      props.modelValue.push(props.value);
    }
  })
  
  watch(props.modelValue, (mv) => {
    if(checked.value && !mv.includes(props.value)) {
      checked.value = false;
    } else if(!checked.value && mv.includes(props.value)) {
      checked.value = true;
    }
  })
</script>




  <style lang="scss">
  @import '@/assets/scss/common/mixins';
  @import '@/assets/scss/common/variables';


  .v-checkbox {
    position: absolute;
    z-index: -1;
    opacity: 0;

    &+label {
      display: inline-flex;
      align-items: center;
      user-select: none;
    }
    &+label::before {
      content: '';
      display: inline-block;
      width: 1em;
      height: 1em;
      flex-shrink: 0;
      flex-grow: 0;
      border: 1px solid #adb5bd;
      border-radius: 0.25em;
      margin-right: 0.5em;
      background-repeat: no-repeat;
      background-position: center center;
      background-size: 50% 50%;
    }
  
    &:checked+label::before {
      border-color: #0b76ef;
      background-color: #0b76ef;
      background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3e%3cpath fill='%23fff' d='M6.564.75l-3.59 3.612-1.538-1.55L0 4.26 2.974 7.25 8 2.193z'/%3e%3c/svg%3e");
    }

    &:not(:disabled):not(:checked)+label:hover::before {
      border-color: #b3d7ff;
    }

    &:not(:disabled):active+label::before {
      background-color: #b3d7ff;
      border-color: #b3d7ff;
    }

    &:focus+label::before {
      box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
    }

    &:focus:not(:checked)+label::before {
      border-color: #80bdff;
    }

    &:disabled+label::before {
      background-color: #e9ecef;
    }
  }
  </style>
   