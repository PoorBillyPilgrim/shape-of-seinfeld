<template>
    <div>
        <p>SEID: {{ SEID }}</p>
    </div>
</template>

<script>
import * as d3 from 'd3'

export default {
    name: 'EpisodeInfo',
    props: {
        SEID: {
            type: String
        }
    },
    methods: {
        async getEpInfo() {
            return await d3.csv(`/data/episode_info.csv`)
        } 
    }
}
</script>

<style>

</style>