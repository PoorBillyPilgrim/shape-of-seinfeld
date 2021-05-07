<template>
    <div class="canvas">
    </div>
</template>

<script>
import * as d3 from 'd3'

export default {
    data() {
        return {
            height: 300,
            width: null,
            margin: {
                top: null,
                right: null,
                bottom: null,
                left: null
            },
            SEID: ''
        }
    },
    created() {
        this.getSeinfeldData()
    },
    methods: {
        async getSEID() {

        },
        async getSeinfeldData() {
            const data = await d3.csv('/data/S01E03.csv');
            let compounds = [],
                count = 0;
            data.forEach(item => {
                if (!item.compound == "") {
                    compounds.push({
                        index: count,
                        compound: +item.compound
                    })
                    count += 1;
                }
            })

            let width = document.querySelector('.canvas').clientWidth;
            
            const x = d3.scaleLinear().domain([compounds[0].index, compounds[compounds.length-1].index]).range([0, 712]); // range corresponds to width of .canvas element
                                            // the domain needs to be the {index} values for the first and last items in data array
            const y = d3.scaleLinear().domain([-1,1]).range([300, 0]) // range corresponds to height of .canvas element
                                            // the [-1, 1] references the 

            const line = d3.line()
                .curve(d3.curveBumpX)
                .x(d => x(d.index))
                .y(d => y(d.compound));
            
            const svg = d3
                .select('.canvas')
                .append('svg')
                .attr('width', width)
                .attr('height', this.height)

            svg.append("path")
                .attr("d", line(compounds))
                .attr("fill", "none")
                .attr("stroke", "rgb(34 150 243)")
                .attr("stroke-width", 1);
            
            const yAxis = d3.axisRight()
                            .scale(y);

            const xAxis = d3.axisBottom(x)
                            .tickFormat(d => (Math.ceil(d/compounds.length*100))+'%')
                            .ticks(2)
                            
                            
                            
            
            svg.append('g')
                .attr("transform", "translate(0,10)")
                .call(yAxis)

            svg.append('g')
                .attr('transform', 'translate(0, 160)')
                .call(xAxis)

            svg.append('line')
                .attr('x1', 0)
                .attr('x2', 100)

            console.log(compounds)
        }
    }
}
</script>

<style scoped>
.canvas {
    width: 50%;
    /* border: solid 1px #2c3e50;*/ 
    margin: 0 auto;
}

</style>