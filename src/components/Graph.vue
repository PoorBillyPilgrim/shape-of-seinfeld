<template>
    <div class="canvas">
    </div>
</template>

<script>
import * as d3 from 'd3'

export default {
    data() {
        return {
            msg: ''
        }
    },
    created() {
        this.getSeinfeldData()
    },
    methods: {
        async getSeinfeldData() {
            const data = await d3.csv('/data/S01E01.csv');
            let compounds = [];
            data.forEach(item => {
                if (!item.compound == "") {
                    compounds.push({
                        index: +item.index,
                        compound: +item.compound
                    })
                }
            })

            let width = document.querySelector('.canvas').clientWidth;
            let height = document.querySelector('.canvas').clientHeight;
            
            const x = d3.scaleLinear().domain([0, data.length]).range([0, 712]);
            const y = d3.scaleLinear().domain([-1,1]).range([300, 0])

            const line = d3.line()
                .x(d => x(d.index))
                .y(d => y(d.compound));
            
            const svg = d3
                .select('.canvas')
                .append('svg')
                .attr('width', width)
                .attr('height', height)

            svg.append("path")
                .attr("d", line(compounds))
                .attr("fill", "none")
                .attr("stroke", "rgb(34 150 243)")
                .attr("stroke-width", 1);

            console.log(compounds)
        }
    }
}
</script>

<style scoped>
.canvas {
    height: 300px;
    width: 50%;
    border: solid 1px #2c3e50;
    margin: 0 auto;
}

</style>