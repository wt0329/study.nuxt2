<template>
  <div>
    <p> parameter : {{parameter }}</p>
  </div>
</template>

<script>
export default {
  // validate(params) {
  //   return /^[a-z]+$/.test(params.paramNum)
  // },
  data() {
    return {parameter : this.$route.params.paramNum}
  },
 // layout : 'main-layout',
}
</script>
