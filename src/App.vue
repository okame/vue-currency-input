<template>
  <input type="text" @blur="blur" v-model="tmp" :id="id" :name="name" />
</template>

<script>
export default {
  name: 'moneyInput',

  props: [
    'value',
    'id',
    'name'
  ],

  data () {
    return {
      tmp: 0
    }
  },

  mounted () {
    this.tmp = this.value
    this.split()
  },

  computed: {
    normalizedValue () {
      return parseInt(String(this.tmp || 0).replace(/,/g, ''), 10)
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
      this.tmp = String(this.tmp).replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1,')
    },
    normalize () {
      this.tmp = parseInt(String(this.tmp || 0).replace(/,/g, ''), 10)
    },
    validate (value) {
      return /^[\d,]*$/.test(value)
    }
  },

  watch: {
    value () {
      this.tmp = this.value
      this.blur()
    },
    tmp (newValue, oldValue) {
      if (!this.validate(newValue)) {
        this.tmp = oldValue
      } else {
        this.split()
        this.$emit('input', this.normalizedValue)
      }
    }
  }
}
</script>
