<template>
  <q-icon name="iconName" />
</template>

<script>
export default {
  name: 'IconForm',
  props: {
    iconName: {
      type: String,
      default: ''
    }
  },
  setup () {
    return {}
  }
}
</script>
