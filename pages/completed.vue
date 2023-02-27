<template>
    <Completed :name="getName" :countyName="getCountyName" :state="getStateName"/>
</template>

<script>
export default {
  computed: {
    getName(){
        return this.$route.params.name
    },
    getCountyName() {
      return this.$route.params.countyName;
    },
    getStateName() {
      return this.$route.params.state;
    }
  }
}
</script>
  