<template>
  <input class="v2i-item"
		:id="id"
		:type="type"
		:ref="keyNode"
		:class="setClassInput"
		:placeholder="placeholder"
		:value="value"

		autocomplete="nope"
		v-model="localValue"

		@input="inputValue($event.target)"
		@blur="$emit('blur', $event.target.value)"
		@focus="$emit('focus', $event.target.value)"
		@keypress.enter="$emit('enter', $event.target.value)"
  >
</template>

<script>

export default {
	name: 'VInput',
	props: [
		'id',
		'value',
		'isFill',
		'keyNode',
		'outerType',
		'placeholder',
		'isPasswordType',
		'isFocusResolution'
	],
	data: () => ({
		type: 'text',
		localValue: ''
	}),
	computed: {
		setClassInput() {
			return this.isPasswordType ? 'v2i-item--password' : null
		}
	},
	methods: {
		inputValue({ value }) {
			this.$emit('input-value', value)

			if (value && this.outerType !== 'text') {
				this.type = 'password'
			}
		}
	},
	watch: {
		outerType(type) {
			this.type = type
		}
	},
	created() {
		this.type = this.isFill
			? 'password' : 'text'
	},
	mounted() {
		const input = this.$refs[this.keyNode]
		
		this.$emit('input-init', input.value)
		if (this.isFocusResolution) {
			input.focus()
		}
	}
}
</script>

<style lang="scss">
	.v2i-item {
		width: 100%;
		height: 100%;
		border: none;
		outline: none;
		border-radius: 8px;
		padding-left: 15px;

		&--password {
			border-top-right-radius: 0;
			border-bottom-right-radius: 0;
		}
	}
</style>