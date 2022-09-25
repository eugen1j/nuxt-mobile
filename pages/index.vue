<template>
  <div>
    <h1>🇺🇦 Kyiv Time</h1>

    <p>
      🕥 Current time: <b>{{ timeData.datetime }}</b>
      <a @click="$nuxt.refresh()" href="">🔄</a>
    </p>
  </div>
</template>

<script setup>
const { data: timeData } = await useFetch('https://worldtimeapi.org/api/timezone/Europe/Kiev')

</script>

