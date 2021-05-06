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
            const data = await d3.csv('/data/S01E03.csv');
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
            
            const x = d3.scaleLinear().domain([data[0].index, data[data.length-1].index]).range([0, 712]); // range corresponds to width of .canvas element
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
                .attr('height', height)

            svg.append("path")
                .attr("d", line(compounds))
                .attr("fill", "none")
                .attr("stroke", "rgb(34 150 243)")
                .attr("stroke-width", 1);
            
            const yAxis = d3.axisRight()
                            .scale(y);

            const xAxis = d3.axisBottom(x)
                            
                            
            
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
    height: 300px;
    width: 50%;
    /* border: solid 1px #2c3e50;*/ 
    margin: 0 auto;
}

</style>