<template>
  <div>
    <h1>πΊπ¦ Kyiv Time</h1>

    <p>
      π₯ Current time: <b>{{ timeData.datetime }}</b>
      <a @click="$nuxt.refresh()" href="">π</a>
    </p>
  </div>
</template>

<script setup>
const { data: timeData } = await useFetch('https://worldtimeapi.org/api/timezone/Europe/Kiev')

</script>

