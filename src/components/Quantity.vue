<template>
  <div :class="['quantity', { disabled: disable }]">
    <button
      type="button"
      class="quantity__adjust quantity__adjust--minus"
      :disabled="disable"
      @click="changeQuantity(-1)"
    >
      <i class="material-icons icon-adjust">keyboard_arrow_down</i>
    </button>
    <input
      type="number"
      class="quantity__num"
      :min="min"
      :max="max"
      :value="internalValue"
      @input="inputValue"
      @blur="blurValue"
    />
    <button
      type="button"
      class="quantity__adjust quantity__adjust--plus"
      :disabled="disable"
      @click="changeQuantity(1)"
    >
      <i class="material-icons icon-adjust">keyboard_arrow_up</i>
    </button>
  </div>
</template>

<script>
import { Quantity } from '../mixins/Quantity'
export default {
  name: 'Quantity',
  mixins: [Quantity],
}
</script>
