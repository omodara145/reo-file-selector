<template>
  <button :class="['reo-button', type, { disabled: disabled }]" @click="$emit('click')" :disabled="disabled">
    <slot />
  </button>
</template>

<script>
export default {
	name: 'ReoButton',
	props: {
		type: {
			type: String,
			default: 'primary'
		},
		disabled: {
			type: Boolean,
			default: false
		}
	},
	data () {
		return {}
	}
}
</script>

<style scoped lang="scss">
.reo-button {
  padding: 10px 16px;
  font-size: 14px;
  border-radius: 4px;
  border: 1px solid transparent;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  color: #ffffff;
  font-family: Avenir, Helvetica, Arial, sans-serif;

  &.primary {
    border-color: #1890FF;
    background: #1890FF;
		transition: all 0.25s ease-in;
  }

  &.disabled {
    color: #BFBFBF;
    border-color: #D9D9D9;
    background: #F5F5F5;
    cursor: not-allowed;
		transition: all 0.25s ease-in;
  }
}
</style>
