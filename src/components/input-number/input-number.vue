<template>
    <div :class="wrapClasses">
        <div :class="handlerClasses">
            <a
                @click="up"
                @mousedown="preventDefault"
                :class="upClasses">
                <span :class="innerUpClasses" @click="preventDefault"></span>
            </a>
            <a
                @click="down"
                @mousedown="preventDefault"
                :class="downClasses">
                <span :class="innerDownClasses" @click="preventDefault"></span>
            </a>
        </div>
        <div :class="inputWrapClasses">
            <input
                :id="elementId"
                :class="inputClasses"
                :disabled="disabled"
                autocomplete="off"
                :autofocus="autofocus"
                @focus="focus"
                @blur="blur"
                @keydown.stop="keyDown"
                @input="change"
                @change="change"
                :readonly="readonly || !editable"
                :name="name"
                :value="precisionValue">
        </div>
    </div>
</template>
<script>
    import { oneOf } from '../../utils/assist';
    import Emitter from '../../mixins/emitter';

    const prefixCls = 'one-input-number';
    const iconPrefixCls = 'one-icon';

    function addNum (num1, num2) {
        let sq1, sq2, m;
        try {
            sq1 = num1.toString().split('.')[1].length;
        }
        catch (e) {
            sq1 = 0;
        }
        try {
            sq2 = num2.toString().split('.')[1].length;
        }
        catch (e) {
            sq2 = 0;
        }
//        if (sq1 === 0 || sq2 === 0) {
//            return num1 + num2;
//        } else {
//            m = Math.pow(10, Math.max(sq1, sq2));
//            return (num1 * m + num2 * m) / m;
//        }
        m = Math.pow(10, Math.max(sq1, sq2));
        return (Math.round(num1 * m) + Math.round(num2 * m)) / m;
    }

    export default {
        name: 'InputNumber',
        mixins: [ Emitter ],
        props: {
            max: {
                type: Number,
                default: Infinity
            },
            min: {
                type: Number,
                default: -Infinity
            },
            step: {
                type: Number,
                default: 1
            },
            value: {
                type: Number,
                default: 1
            },
            size: {
                validator (value) {
                    return oneOf(value, ['small', 'large', 'default']);
                }
            },
            disabled: {
                type: Boolean,
                default: false
            },
            autofocus: {
                type: Boolean,
                default: false
            },
            readonly: {
                type: Boolean,
                default: false
            },
            editable: {
                type: Boolean,
                default: true
            },
            name: {
                type: String
            },
            precision: {
                type: Number
            },
            elementId: {
                type: String
            }
        },
        data () {
            return {
                focused: false,
                upDisabled: false,
                downDisabled: false,
                currentValue: this.value
            };
        },
        computed: {
            wrapClasses () {
                return [
                    `${prefixCls}`,
                    {
                        [`${prefixCls}-${this.size}`]: !!this.size,
                        [`${prefixCls}-disabled`]: this.disabled,
                        [`${prefixCls}-focused`]: this.focused
                    }
                ];
            },
            handlerClasses () {
                return `${prefixCls}-handler-wrap`;
            },
            upClasses () {
                return [
                    `${prefixCls}-handler`,
                    `${prefixCls}-handler-up`,
                    {
                        [`${prefixCls}-handler-up-disabled`]: this.upDisabled
                    }
                ];
            },
            innerUpClasses () {
                return `${prefixCls}-handler-up-inner ${iconPrefixCls} ${iconPrefixCls}-ios-arrow-up`;
            },
            downClasses () {
                return [
                    `${prefixCls}-handler`,
                    `${prefixCls}-handler-down`,
                    {
                        [`${prefixCls}-handler-down-disabled`]: this.downDisabled
                    }
                ];
            },
            innerDownClasses () {
                return `${prefixCls}-handler-down-inner ${iconPrefixCls} ${iconPrefixCls}-ios-arrow-down`;
            },
            inputWrapClasses () {
                return `${prefixCls}-input-wrap`;
            },
            inputClasses () {
                return `${prefixCls}-input`;
            },
            precisionValue () {
                // can not display 1.0
                return this.precision ? this.currentValue.toFixed(this.precision) : this.currentValue;
            }
        },
        methods: {
            preventDefault (e) {
                e.preventDefault();
            },
            up (e) {
                const targetVal = Number(e.target.value);
                if (this.upDisabled && isNaN(targetVal)) {
                    return false;
                }
                this.changeStep('up', e);
            },
            down (e) {
                const targetVal = Number(e.target.value);
                if (this.downDisabled && isNaN(targetVal)) {
                    return false;
                }
                this.changeStep('down', e);
            },
            changeStep (type, e) {
                if (this.disabled || this.readonly) {
                    return false;
                }

                const targetVal = Number(e.target.value);
                let val = Number(this.currentValue);
                const step = Number(this.step);
                if (isNaN(val)) {
                    return false;
                }

                // input a number, and key up or down
                if (!isNaN(targetVal)) {
                    if (type === 'up') {
                        if (addNum(targetVal, step) <= this.max) {
                            val = targetVal;
                        } else {
                            return false;
                        }
                    } else if (type === 'down') {
                        if (addNum(targetVal, -step) >= this.min) {
                            val = targetVal;
                        } else {
                            return false;
                        }
                    }
                }

                if (type === 'up') {
                    val = addNum(val, step);
                } else if (type === 'down') {
                    val = addNum(val, -step);
                }
                this.setValue(val);
            },
            setValue (val) {
                // 如果 step 是小数，且没有设置 precision，是有问题的
                if (!isNaN(this.precision)) val = Number(Number(val).toFixed(this.precision));

                this.$nextTick(() => {
                    this.currentValue = val;
                    this.$emit('input', val);
                    this.$emit('on-change', val);
                    this.dispatch('FormItem', 'on-form-change', val);
                });
            },
            focus () {
                this.focused = true;
            },
            blur () {
                this.focused = false;
            },
            keyDown (e) {
                if (e.keyCode === 38) {
                    e.preventDefault();
                    this.up(e);
                } else if (e.keyCode === 40) {
                    e.preventDefault();
                    this.down(e);
                }
            },
            change (event) {
                let val = event.target.value.trim();

                if (event.type == 'input' && val.match(/^\-?\.?$|\.$/)) return; // prevent fire early if decimal. If no more input the change event will fire later
                if (event.type == 'change' && Number(val) === this.currentValue) return; // already fired change for input event

                const {min, max} = this;
                const isEmptyString = val.length === 0;
                val = Number(val);
                if (!isNaN(val) && !isEmptyString) {
                    this.currentValue = val;

                    if (event.type == 'input' && val < min) return; // prevent fire early in case user is typing a bigger number. Change will handle this otherwise.
                    if (val > max) {
                        this.setValue(max);
                    } else if (val < min) {
                        this.setValue(min);
                    } else {
                        this.setValue(val);
                    }
                } else {
                    event.target.value = this.currentValue;
                }
            },
            changeVal (val) {
                val = Number(val);
                if (!isNaN(val)) {
                    const step = this.step;

                    this.upDisabled = val + step > this.max;
                    this.downDisabled = val - step < this.min;
                } else {
                    this.upDisabled = true;
                    this.downDisabled = true;
                }
            }
        },
        mounted () {
            this.changeVal(this.currentValue);
        },
        watch: {
            value (val) {
                this.currentValue = val;
            },
            currentValue (val) {
                this.changeVal(val);
            },
            min () {
                this.changeVal(this.currentValue);
            },
            max () {
                this.changeVal(this.currentValue);
            }
        }
    };
</script>
