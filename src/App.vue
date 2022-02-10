<template>
  <Layout>
    <main>
      <h1>Live Stream Schedule</h1>
      <p>This is a list of upcoming and past live coding sessions. If you would like to see me talk about a particular subject please reach out to me on <a href="https://www.twitter.com/therealdanvega">Twitter</a>.</p>

      <h2>Upcoming Streams</h2>
      <LiveStreamTable :streams="upcomingStreams"/>

      <h2>Archived Streams</h2>
      <LiveStreamTable :streams="archivedStreams"/>

    </main>
  </Layout>
</template>

<script setup>
import LiveStreamTable from './components/LiveStreamTable.vue'
import { ref, onMounted, computed } from 'vue'

let streams = ref([]);

const upcomingStreams = computed(() => {
  return streams.value.filter(stream => new Date(stream.startDate) >= new Date());
})

const archivedStreams = computed(() => {
  return streams.value.filter(stream => new Date(stream.startDate) < new Date());
})

onMounted(() => {
  fetch('/.netlify/functions/streams')
  .then(res => res.json())
  .then(data => {
    streams.value = data;
  });
})
</script>
