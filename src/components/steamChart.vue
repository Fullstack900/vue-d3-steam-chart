<template>
    <div class="clip-chart-container">
        <svg id="chart" />
    </div>
</template>

<script>
    import * as d3 from 'd3';
    export default {
        props: {
            dataObject: {
                type: Object,
                required: true
            },
            config: {
                type: Object,
                required: true
            }
        },

        mounted () {
            this.generateSteamChart();
        },
        methods: {

            drawWeekOnWeekText(type, y, dy1, dy2, dy3, data) {
                d3
                .selectAll('#chart > g > g.top-x-axis-container > g.top-x-axis')
                .data(data)
                .append('text')
                .attr('y', y)
                .attr('dy', dy1)
                .attr('fill', 'black')
                .text(d => {
                    if(type === 'high') {
                        return `High Risk: ${d.highRisk.value}`;
                    } else if(type === 'medium') {
                        return `Medium Risk: ${d.mediumRisk.value}`;
                    } else {
                        return `Low Risk: ${d.lowRisk.value}`;
                    }
                    })
                .style('font-size', '12px')
                .style('color', '#2c3e50');

                 // .html(d => {
                    //     return 'Low Risk:' + `${this.(d.lowRisk.value, 'amount', 'short')}`
                    // })

            // Top Caret Icon
               d3
                    .selectAll('#chart > g > g.top-x-axis-container > g.top-x-axis')
                    .data(data)
                    .append("text")
                    .attr("font-family","FontAwesome")
                    .attr("font-size", "14px")
                    .attr('y', y)
                    .attr('dy', dy2)
                    .attr('x', 50)
                    .text(function(d) {
                        if(type === 'high') {
                            if (d.highRisk.direction == 'up' || d.highRisk.direction == 'no-change'){
                                return "\uf0d8"
                            } else if(d.highRisk.direction == 'down'){
                                return "\uf0d7"
                            }
                        } else if(type === 'medium') {
                            if (d.mediumRisk.direction == 'up' || d.mediumRisk.direction == 'no-change'){
                                return "\uf0d8"
                            } else if(d.mediumRisk.direction == 'down'){
                                return "\uf0d7"
                            }
                        } else if(type === 'low') {
                            if (d.lowRisk.direction == 'up' || d.lowRisk.direction == 'no-change'){
                                return "\uf0d8"
                            } else if(d.lowRisk.direction == 'down'){
                                return "\uf0d7"
                            }
                        }
                    })
                    .attr("fill", function(d) {
                        if(type === 'high') {
                            if (d.highRisk.direction == 'up'){
                                return "green";
                            } else if (d.highRisk.direction == 'no-change'){
                                return "gold";
                            } else if(d.highRisk.direction == 'down'){
                                return "red";
                            }
                        } else if(type === 'medium') {
                            if (d.mediumRisk.direction == 'up'){
                                return "green";
                            } else if (d.mediumRisk.direction == 'no-change'){
                                return "gold";
                            } else if(d.mediumRisk.direction == 'down'){
                                return "red";
                            }
                        } else if(type === 'low') {
                            if (d.lowRisk.direction == 'up'){
                                return "green";
                            } else if (d.lowRisk.direction == 'no-change'){
                                return "gold";
                            } else if(d.lowRisk.direction == 'down'){
                                return "red";
                            }
                        }
                    })
                    .attr('class', 'top-caret-icon')
                    ;

                // Top Caret Icon Tooltip
                console.log("here is data", data);
                d3.select('body').append('rect').attr('id', 'tooltip').attr('style', 'position: absolute; opacity: 0;');
                    // d3.select('body').append('svg').attr('width', 300).attr('height', 300);
                    // let caretData = data;
                    let caretUpKeys = data.map((columnKey) => {
                        let columnKeys = Object.keys(columnKey);
                        return columnKeys;
                    });
                    caretUpKeys = caretUpKeys.flat();

                    let caretUpValues = data.map( (columnValue) => {
                    let columnValues = Object.values(columnValue);
                    return columnValues;
                    });

                    caretUpValues = caretUpValues.flat().map(caretUpValue => caretUpValue.value);

                    let caretData = [];
                    for (let iterator = 0; iterator < caretUpKeys.length; iterator++){
                        caretData[iterator] = caretUpKeys[iterator] + " " + caretUpValues[iterator];
                    }

                    d3.select('svg').selectAll('#chart > g > g.top-x-axis-container > g.top-x-axis > text.top-caret-icon').data(caretData)
                    // .join('circle')
                    .attr('r', 3)
                    .attr('cy', 5)
                    .attr('cx', (d,i) => i*15+15)
                    .style('cursor','pointer')
                    .on('mouseover', function(event) {
                        d3.select('#tooltip').transition().duration(200).style('opacity', 1).text(event.target.previousElementSibling.innerHTML)
                        .style('color', '#ffffff')
                        .style('background', '#151b42')
                        .style('padding', '1rem')
                        .style('border-radius', '0.5rem')
                        .style('transform', 'scale(0.8)');
                        // Below is for the other div
                        // .append('rect')
                        // .attr('height', '1rem')
                        // .attr('width', '1rem')
                        // .style('background', '#151b42')
                        // .append('text').attr('class', 'fa anchor-start arrowIcon');
                        // .text()
                        // .attr('class', 'fa anchor-start arrowIcon');
                        // d3.select('#tooltip').append('text').attr('class', 'fa anchor-start arrowIcon');
                        // .text('\uf0d7');
                    })
                    .on('mouseout', function() {
                        d3.select('#tooltip').style('opacity', 0)
                    })
                    .on('mousemove', function(event) {
                        d3.select('#tooltip').style('left', (event.pageX + 10) + 'px').style('top', (event.pageY - 24) + 'px')
                    })

            // d3.select("#chart > g > g.top-x-axis-container > g.top-x-axis > text.top-caret-icon")

            // Bottom Caret Icon
                d3
                    .selectAll('#chart > g > g.top-x-axis-container > g.top-x-axis')
                    .data(data)
                    .append("text")
                    .attr("font-family","FontAwesome")
                    .attr("font-size", "14px")
                    .attr('y', y)
                    .attr('dy', dy3)
                    .attr('x', 50)
                    .text(function(d) {
                        if(type === 'high') {
                            if (d.highRisk.direction == 'no-change'){
                                return "\uf0d7"
                            }
                        } else if(type === 'medium') {
                            if (d.mediumRisk.direction == 'no-change'){
                                return "\uf0d7"
                            }
                        } else {
                            if(type === 'low') {
                            if (d.lowRisk.direction == 'no-change'){
                                return "\uf0d7"
                            }
                        }
                        }
                    })
                    .attr("fill", function(d) {
                        if(type === 'high') {
                            if (d.highRisk.direction == 'no-change'){
                                return "gold";
                            }
                        } else if(type === 'medium') {
                            if (d.mediumRisk.direction == 'no-change'){
                                return "gold";
                            }
                        } else {
                            if (d.lowRisk.direction == 'no-change'){
                                return "gold";
                            }
                        }
                    });

            },
            generateSteamChart () {
                const margin = { top: 80, bottom: 20, left: 42, right: -29 };
                const width = window.innerWidth;
                const height = 530 - margin.top - margin.bottom;

                // define svg
                const svg = d3
                    .select('svg#chart')
                    .attr('width', width)
                    .attr('height', height + margin.top + margin.bottom + 30)
                    .style('background-color', '#F7F9FC');

                const gChart = svg
                    .append('g')
                    .attr('transform', 'translate(0,120) scale(1.14 1)');

                const addGrayScale = function () {
                    gChart.selectAll('path').style('filter', 'grayscale(0.9)');
                };

                const removeGrayScale = function () {
                    gChart.selectAll('path').style('filter', 'grayscale(0)');
                };

                // const chartData = this.dataObject;
                const chartData = {
                    graph: {
                    coordinates: [
                        {
                        heading: "Not Qualified",
                        History: 216,
                        BestCase: 2408,
                        WorstCase: 2646,
                        WinThreshold: 1488
                        },

                        {
                        heading: "Closed Lost",
                        History: 1323,
                        BestCase: 3322,
                        WorstCase: 2389,
                        WinThreshold: 1074
                        },

                        {
                        heading: "Prospect",
                        History: 604,
                        BestCase: 4657,
                        WorstCase: 6383,
                        WinThreshold: 2335
                        },

                        {
                        heading: "Tech Validation",
                        History: 1139,
                        BestCase: 4847,
                        WorstCase: 2687,
                        WinThreshold: 2545
                        },

                        {
                        heading: "Closed Won",
                        History: 1204,
                        BestCase: 5612,
                        WorstCase: 2062,
                        WinThreshold: 3219
                        },

                        {
                        heading: "Proposed",
                        History: 1244,
                        BestCase: 5389,
                        WorstCase: 2380,
                        WinThreshold: 3211
                        },

                        {
                        heading: "Present & Quality",
                        History: 1126,
                        BestCase: 3231,
                        WorstCase: 3281,
                        WinThreshold: 2599
                        },

                        {
                        heading: "",
                        History: 2226,
                        BestCase: 2451,
                        WorstCase: 2281,
                        WinThreshold: 5299
                        }
                    ],
                    weekOnWeek: [
                        {
                        highRisk:{
                            value: 22,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 4,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        },
                        {
                        highRisk:{
                            value: 23,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 2,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        },

                        {
                        highRisk:{
                            value: 223,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 4,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        },

                        {
                        highRisk:{
                            value: 123,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 4,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        },

                        {
                        highRisk:{
                            value: 235,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 4,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        },

                        {
                        highRisk:{
                            value: 436,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 4,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        },

                        {
                        highRisk:{
                            value: 22,
                            direction: "up"
                        },
                        mediumRisk: {
                            value: 4,
                            direction: "down"
                        },
                        lowRisk: {
                            value: 3.2,
                            direction: "no-change"
                        }
                        }
                    ]
                    }
                };

                const jsonData = chartData.graph.coordinates;

                const headings = jsonData.map(d => d.heading);
                const bizUnits = Object.keys(jsonData[0]).filter(d => d != 'heading');
                const colors = ['#ff7e81', '#ffb681', '#ffee62', '#8bcd69'];
                // const colors = this.config.colors;

                const fillScale = d3
                    .scaleOrdinal()
                    .domain(bizUnits)
                    .range(colors);

                // x axis
                const xScale = d3
                    .scaleBand()
                    .domain(headings)
                    .range([0, width]);
                const xAxis = d3.axisBottom(xScale).tickSize(-height + 15);

                // stack layout
                const steam = d3
                    .stack()
                    .offset(d3.stackOffsetSilhouette)
                    .order(d3.stackOrderInsideOut)
                    .keys(bizUnits)(jsonData);

                // x and y values returned by layout
                const allCoords = steam
                    .map(d => {
                        const arr = d.reduce((acc, curVal) => acc.concat(curVal), []);
                        return arr;
                    })
                    .reduce((acc, curVal) => acc.concat(curVal), []);

                const yScale = d3
                    .scaleLinear()
                    .domain(d3.extent(allCoords))
                    .range([height, 0]);

                // area generator
                const stackArea = d3
                    .area()
                    .x(d => xScale(d.data.heading))
                    .y0(d => yScale(d[0]))
                    .y1(d => yScale(d[1]))
                    .curve(d3.curveBasis);

                // add area paths
                gChart
                    .selectAll('path')
                    .data(steam)
                    .enter()
                    .append('path')
                    .style('fill', d => fillScale(d.key))
                    .attr('d', d => stackArea(d));

                gChart.selectAll('path').on('click', function (event) {
                    if(event.target.style.filter === 'grayscale(0%)'){
                      removeGrayScale();
                    }
                    else{
                      addGrayScale();
                      event.target.style.filter = 'grayscale(0%)';
                    }
                });

                // add bottom x axis
                gChart
                    .append('g')
                    .attr('transform', `translate(${-115},${542}) scale(1 1.7)`)
                    .call(xAxis)
                    .attr('class', 'bottom-x-axis-container')
                    .selectAll('g')
                    .attr('class', 'bottom-x-axis')
                    .select('line')
                    .attr('stroke-dasharray', '10')
                    .attr('stroke-opacity', '0.4');

                // add top x axis
                gChart
                    .append('g')
                    .attr('transform', 'translate(-5,-95)')
                    .call(xAxis.tickSize(0))
                    .attr('class', 'top-x-axis-container')
                    .selectAll('g')
                    .attr('class', 'top-x-axis');

                d3
                    .selectAll('#chart > g > g.top-x-axis-container > g.top-x-axis')
                    .select('text')
                    .style('font-size', '16px')
                    .style('font-weight', '900')
                    .style('color', '#000000');

                // Add High-Risk Data to Columns
                this.drawWeekOnWeekText('high', 3, '3.20em', '2.70em', '3.20em', chartData.graph.weekOnWeek);

                // Add Medium-Risk Data to Columns
                this.drawWeekOnWeekText('medium', 4, '5.20em', '4.35em', '4.85em', chartData.graph.weekOnWeek);

                // Add Low-Risk Data to Columns
                this.drawWeekOnWeekText('low', 5, '7.20em', '5.90em', '6.40em', chartData.graph.weekOnWeek);

                // This removes the Horizontal Lines of the X-Axis & Y-Axis
                d3
                    .selectAll('#chart > g > g')
                    .select('path')
                    .remove();

                // This remove the Labels from the bottom X-Axis
                d3
                    .selectAll('#chart > g > g.bottom-x-axis-container > g.bottom-x-axis')
                    .select('text')
                    .remove();

                d3
                    .select(
                        '#chart > g > g.bottom-x-axis-container > g.bottom-x-axis:first-child'
                    )
                    .remove();
                d3
                    .select(
                        '#chart > g > g.bottom-x-axis-container > g.bottom-x-axis:last-child'
                    )
                    .remove();

                // This will remove the last Label from the top X-Axis:
                d3
                    .select('#chart > g > g.top-x-axis-container > g.top-x-axis:last-child')
                    .remove();
            },

        }
    };
</script>

<style scoped lang="css">
.clip-chart-container {
  /* overflow-x: scroll; */
  overflow-x: hidden;
  border: 2px solid black;
  border-radius: 20px !important;
}

rect#tooltip{
    font-size: 0.75rem;
}

.anchor-start {
  text-anchor: start;
}

.arrowIcon {
fill: #415266;
font-size: 1.5rem;
}

</style>
