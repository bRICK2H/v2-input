<template>
    <div class="v2i-wrap">

        <!-- Label -->
        <label v-show="label"
            class="v2i-label"
            :for="inputKey"
        >
            {{ label }}

            <span class="v2i-required"
                v-show="required || required === ''"
            >*</span>
        </label>
        
        <!-- Input container -->
        <div class="v2i-container"
            :class="[target, { 'v2i-container--focus': isFocus }]"
            :style="setStyleInputConitaner"
        >
            <!-- Input -->
            <VInput
                :id="inputKey"
                :value="value"
                :key="inputKey"
                :keyNode="inputKey"
                :isFill="isFill"
                :outerType="getOuterType"
                :placeholder="placeholder"
                :isPasswordType="isPasswordType"
                :isFocusResolution="isFocusResolution"

                @input-init="inputInit"
                @input-value="inputValue"
                @blur="blur"
                @focus="focus"
                @enter="value => $emit('enter', value)"
            />

            <!-- Passsorw icon -->
            <span v-show="isPasswordType"
                class="v2i-icon"
                :class="setClassIsDisabledShowPassword"
                @click="isDisabledShowPassword ? false : isShowPassword = !isShowPassword"
            >
                <img :src="getIcon" alt="eye">
            </span>
        </div>
    </div>
</template>

<script>
import VInput from './components/v-input.vue'

export default {
    components: {
        VInput
    },
    props: {
        value : null,
        required: null,
        type: {
            type: String,
            default: 'text'
        },
        label: {
            type: String,
            default: ''
        },
        placeholder: {
            type: String,
            default: ''
        },
        target: {
            type: String,
            default: ''
        },
        height: {
            type: [String, Number],
            default: 48
        },
        isFill: {
            type: Boolean,
            default: false
        },
        isLoadFocus: {
            type: Boolean,
            default: false
        },
    },
    data: function () {
        return {
            isFocus: false,
            isShowPassword: false,
            isFocusResolution: false,
            isDisabledShowPassword: true,
            inputKey: String(Math.random()).slice(2, 10),
        }
    },
    computed: {
        isPasswordType() {
            return this.type === 'password'
        },
        getOuterType() {
            return this.isPasswordType
                ? this.isShowPassword ? 'text' : 'password' : this.type
        },
        getIcon() {
            const name = this.isShowPassword ? 'eye-open' : 'eye-close'

            return require(`./assets/img/svg/${name}.svg`)
        },
        setClassIsDisabledShowPassword() {
            return this.isDisabledShowPassword ? 'v2i-icon__disabled' : null
        },
        setStyleInputConitaner() {
            return {
                height: `${this.height}px`,
                paddingRight: this.isPasswordType ? '50px' : 0
            }
        },
    },
    methods: {
        inputInit(value) {
            this.isDisabledShowPassword = !value
        },
        inputValue(value) {
            this.isDisabledShowPassword = !value

            if (!value) {
                if (!this.isFill) {
                    this.inputKey = String(Math.random()).slice(2, 10)
                }

                this.isShowPassword = false
            }
            
            this.$emit('input', value)
        },
        focus(value) {
            this.isFocus = true
            this.$emit('focus', value)
        },
        blur(value) {
            this.isFocus = false
            this.$emit('blur', value)
        }
    },
    watch: {
        inputKey: {
            immediate: true,
            handler(newVal, oldVal) {
                this.isFocusResolution =
                    (oldVal === undefined && this.isLoadFocus)
                    || typeof newVal === 'string' && typeof oldVal === 'string'
                        ? true : false
            }
        }
    },
}
</script>

<style lang="scss">
    .v2i-wrap {
        width: 100%;
    }
    .v2i-label {
        display: inline-block;
        font-size: 14px;
        font-weight: 600;
        margin-bottom: 8px;
    }
    .v2i-required {
        color: #ec4848;
    }
    .v2i-container {
        height: 56px;
        border: 2px solid #eeedf7;
        border-radius: 8px;
        position: relative;
        transition: border .2s;

        &--focus {
            border: 2px solid #d6d5dd;
        }
    }
    .v2i-icon {
        position: absolute;
        top: 50%;
        right: 13px;
        transform: translateY(-50%);
        cursor: pointer;
        opacity: .6;
        transition: .2s;

        &:hover {
            opacity: 1;
        }

        &__disabled {
            opacity: .3;
            cursor: no-drop;

            &:hover {
                opacity: .3;
            }
        }
    }
</style>