<template>
  <div id="app">
    <CityList/>
  </div>
</template>

<script>
import CityList from "@/components/cityList";

export default {
  name: 'app',
  components: {
    CityList
  }
}
</script>
<style>
#app{
  width: 100%;
  height: 100%;
}
</style>
