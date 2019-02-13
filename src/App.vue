<template>
  <input type="text" @blur="blur" v-model="visibleValue" :id="id" :name="name" />
</template>

<script>
export default {
  name: 'VueCurrencyInput',

  props: {
    value: {
      default: 0,
      type: Number
    },
    id: {
      default: null,
      type: String
    },
    name: {
      default: null,
      type: String
    }
  },

  data () {
    return {
      visibleValue: 0
    }
  },

  mounted () {
    this.visibleValue = this.value
    this.split()
  },

  computed: {
    normalizedValue () {
      return parseInt(String(this.visibleValue || 0).replace(/,/g, ''), 10)
    }
  },

  methods: {
    blur () {
      this.emit()
    },
    emit () {
      this.$emit('input', this.normalizedValue)
    },
    split () {
      this.normalize()
      this.visibleValue = String(this.visibleValue).replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1,')
    },
    normalize () {
      this.visibleValue = parseInt(String(this.visibleValue || 0).replace(/,/g, ''), 10)
    },
    validate (value) {
      return /^[\d,]*$/.test(value)
    }
  },

  watch: {
    value () {
      this.visibleValue = this.value
      this.blur()
    },
    visibleValue (newValue, oldValue) {
      if (!this.validate(newValue)) {
        this.visibleValue = oldValue
      } else {
        this.split()
        this.$emit('input', this.normalizedValue)
      }
    }
  }
}
</script>
