<template>
    <div class="graph">
    </div>
</template>

<script>
import * as d3 from 'd3'

export default {
    name: 'EpisodeGraph',
    data() {
        return {
            height: 500,
            width: 800,
            margin: {
                top: 50,
                right: 50,
                bottom: 50,
                left: 50
            }
        }
    },
    props: {
        SEID: {
            type: String,
        }
    },
    created() {
        this.drawGraph()
    },
    watch: {
        SEID: {
            handler() {
                this.resetGraph()
            },
            immediate: true,
            deep: true
        }
    },
    methods: {
        async drawGraph() {
            const data = await d3.csv(`/data/${this.SEID}.csv`);
            let compounds = [],
                count = 0;
            data.forEach(item => {
                if (!item.compound == "") {
                    compounds.push({ index: count, compound: +item.compound })
                    count += 1;
                }
            })
            
            const x = d3.scaleLinear()
                        .domain([compounds[0].index, compounds[compounds.length-1].index])
                        .range([0, this.width]); 
                        // range corresponds to width of .canvas element
                        // the domain needs to be the {index} values for the first and last items in data array
            const y = d3.scaleLinear()
                        .domain([-1,1])
                        .range([this.height, 0]) 
                        // range corresponds to height of .canvas element
                        // the [-1, 1] references the 

            const line = d3.line()
                .curve(d3.curveBumpX)
                .x(d => x(d.index))
                .y(d => y(d.compound));
            
            const svg = d3
                .select('.graph')
                .append('svg')
                .attr('width', this.width + this.margin.right + this.margin.left)
                .attr('height', this.height + this.margin.top  + this.margin.bottom)
                .append('g')
                .attr('transform', `translate(0, ${this.margin.top})`)

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
                .call(yAxis)

            svg.append('g')
                .attr('transform', `translate(0, ${this.height/2})`)
                .call(xAxis)
        },
        removeGraph() {
            d3.select('svg').remove();
        },
        resetGraph() {
            this.removeGraph()
            this.drawGraph()
        }
    }
}
</script>

<style scoped>
.graph {
    width: 100%;
    /* border: solid 1px #2c3e50;*/ 
    margin: 0 auto;
}
</style>