
<script>
export default {
  name: 'WdRadio',
  inject: ['radioGroup'],
  props: {
    value: [String, Number, Boolean],
    shape: String,
    checkedColor: String,
    maxWidth: String,
    disabled: Boolean
  },
  computed: {
    isChecked () {
      return this.radioGroup.value === this.value
    },
    finalShape () {
      return this.shape || this.radioGroup.shape
    },
    finalCheckedColor () {
      return this.checkedColor || this.radioGroup.checkedColor
    },
    finalDisabled () {
      return this.disabled || this.radioGroup.disabled
    },
    inline () {
      return this.radioGroup.inline
    },
    size () {
      return this.radioGroup.size || ''
    }
  },
  methods: {
    handleChange () {
      this.radioGroup.changeValue(this.value)
    }
  },

  render (h) {
    const {
      isChecked,
      finalShape,
      value,
      finalDisabled,
      finalCheckedColor,
      maxWidth,
      size,
      inline
    } = this

    const label = (
      <div
        class="wd-radio__label"
        style={{
          color: isChecked && finalShape === 'button' && !finalDisabled ? finalCheckedColor : '',
          maxWidth: maxWidth
        }}>{this.$slots.default}</div>
    )
    const input = (
      <input
        type="radio"
        class="wd-radio__input"
        value={value}
        checked={isChecked}
        disabled={finalDisabled}
        onChange={this.handleChange}
      />
    )
    const checkIcon = finalShape === 'check' ? (
      <i class="wd-icon-check" style={{ color: isChecked && !finalDisabled ? finalCheckedColor : '' }}></i>
    ) : ''
    const shape = (
      <span
        class="wd-radio__shape"
        style={{ color: isChecked && !finalDisabled ? finalCheckedColor : '' }}>
        {checkIcon}{input}
      </span>
    )
    // 单选框
    const classList = {
      'wd-radio': true,
      'is-checked': isChecked,
      'is-large': size && (size === 'large'),
      'is-button': finalShape === 'button',
      'is-dot': finalShape === 'dot',
      'is-disabled': finalDisabled,
      'is-inline': inline
    }
    const radio = inline ? (
      <label
        class={{
          ...classList,
          'is-inline': inline
        }}>
        {shape}{label}
      </label>
    ) : (
      <label
        class={{ ...classList }}>
        {label}{shape}
      </label>
    )
    return radio
  }
}
</script>
