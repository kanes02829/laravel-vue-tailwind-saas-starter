<template>
  <div class="relative" :class="marginBottom">
    <input :id="id?id:name" v-model="compVal" :disabled="disabled"
           type="color"
           :name="name"
    >
    <label v-if="label" :for="id?id:name" class="text-gray-700 dark:text-gray-300">
      {{ label }}
      <span v-if="required" class="text-red-500 required-dot">*</span>
    </label>
    <has-error v-if="form" :form="form" :field="name"/>
  </div>
</template>

<script>
import inputMixin from '~/mixins/forms/input'

export default {
  name: 'ColorInput',

  mixins: [inputMixin],

  props: {
    nativeType: {type: String, default: 'text'},
  },
}
</script>
