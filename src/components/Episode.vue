<template>
    <div>
        <button @click="returnRandomSEID()">Random Episode</button>
        <div class="row">
            <EpisodeGraph :SEID="SEID" />
            <EpisodeInfo class="col" :SEID="SEID"/>
        </div>
    </div>
</template>

<script>
import EpisodeGraph from '@/components/EpisodeGraph.vue'
import EpisodeInfo from '@/components/EpisodeInfo.vue'

export default {
  name: 'Episode',
  components: {
    EpisodeGraph,
    EpisodeInfo
  },
  data() {
      return {
          SEID: ''
      }
  },
  methods: {
        getRandomInt(max) {
            return Math.floor(Math.random() * max);
        },
        returnRandomSEID() {
            //const epInfo = await this.getEpInfo();
            //const seasonIds = Array.from(new Set(epInfo.map((item) => item.SEID)))
            let seasonIds = ['S01E01', 'S01E02', 'S01E03']; // for testing
            let randomInt = this.getRandomInt(seasonIds.length)
            this.SEID = seasonIds[randomInt]
        },
  },
  created() {
      this.returnRandomSEID()
  }
}
</script>

<style scoped>

</style>