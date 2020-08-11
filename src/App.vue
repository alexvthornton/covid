<template>
    <div id="app">
        <div id="header" class="col text-center">
    
            <div class="col text-center">
                <h1>COVID-19 Data Visualization</h1>
    
            </div>
    
        </div>
    
        <body id="body" class="container">
            <div class="row mt-5" v-if="arrDeaths.length > 0">
                <div class="col">
                    <h2>Overlays</h2>
    
                    <section class="col text-center">
                        <ul id="checkboxes">
                            <li><input type="checkbox" id="positive" value="positive" name="graphs" v-model="checkedGraph">
                                <label for="positive">Positive</label></li>
    
                            <li><input type="checkbox" id="hospitalized" value="hospitalized" name="graphs" v-model="checkedGraph">
                                <label for="hospitalized">Hospitalized</label></li>
    
                            <li><input type="checkbox" id="icu" value="icu" name="graphs" v-model="checkedGraph">
                                <label for="icu">In ICU</label></li>
    
                            <li><input type="checkbox" id="ventilators" value="ventilators" name="graphs" v-model="checkedGraph">
                                <label for="ventilators">On Ventilators</label></li>
    
                            <li><input type="checkbox" id="recovered" value="recovered" name="graphs" v-model="checkedGraph">
                                <label for="recovered">Recovered</label></li>
    
                            <li><input type="checkbox" id="death" value="death" name="graphs" v-model="checkedGraph">
                                <label for="death">Death</label></li>
    
                        </ul>
    
                        <br>
                        <div id="main-chart">
                            <multi-data-line-chart id="myChart" :key="checkedGraph" :chartData="graphData()" :options="chartOptions" :labels="graphLabel()" :chartColors="graphColors()"></multi-data-line-chart>
                        </div>
                    </section>
    
                    <br>
                </div>
                <!-- need fisrtChart() and secondChart() to be called every time checkedGraph changes maybe with watcher idk-->
            </div>
        </body>
    </div>
</template>

<script>
import axios from 'axios'; // what we will use to make calls to API
import moment from 'moment'; // for date formating

import multiDataLineChart from "./components/multiDataLineChart"

export default {
    name: 'App',
    components: {
        multiDataLineChart
    },
    data() {
        return {
            arrPositive: [],
            arrhospitalized: [],
            arrInIcu: [],
            arrOnVentilators: [],
            arrRecovered: [],
            arrDeaths: [],

            chartOptions: {
                responsive: true,
                maintainAspectRation: false,

            },

            checkedGraph: ["hospitalized", "death"],
            checkedName: true,

            allGraphColors: [{
                    borderColor: "#272f87",
                    pointDorderColor: "#0a1b2e",
                    pointBackgroundColor: "#AFD6AC",
                    backgroundColor: "#FFFF6650"
                },
                {
                    borderColor: "#077187",
                    pointDorderColor: "#0E1428",
                    pointBackgroundColor: "#AFD6AC",
                    backgroundColor: "#FF7F5050"
                },
                {
                    borderColor: "#077187",
                    pointDorderColor: "#0E1428",
                    pointBackgroundColor: "",
                    backgroundColor: "#AFD6AC80"
                },
                {
                    borderColor: "#381A1670",
                    pointDorderColor: "#470D06B0",
                    pointBackgroundColor: "#D05E51B0",
                    backgroundColor: "#941B0D50"
                },
                {
                    borderColor: "#077187",
                    pointDorderColor: "#0E1428",
                    pointBackgroundColor: "#AFD6AC",
                    backgroundColor: "#0716B850"
                },
                {
                    borderColor: "#077187",
                    pointDorderColor: "#0E1428",
                    pointBackgroundColor: "#AFD6AC",
                    backgroundColor: "#74A57F80"
                }
            ]

        }
    },
    async created() {
        const { data } = await axios.get("https://covidtracking.com/api/us/daily");
        data.forEach(d => {
            const date = moment(d.date, "YYYYMMDD").format("MM/DD");

            const {
                positive,
                hospitalizedCurrently,
                inIcuCurrently,
                onVentilatorCurrently,
                recovered,
                death
            } = d;

            this.arrPositive.push({ date, total: positive });
            this.arrhospitalized.push({ date, total: hospitalizedCurrently });
            this.arrInIcu.push({ date, total: inIcuCurrently });
            this.arrOnVentilators.push({ date, total: onVentilatorCurrently });
            this.arrRecovered.push({ date, total: recovered });
            this.arrDeaths.push({ date, total: death });
        })
    },
    methods: {
        graphData() {

            let graphDataArr = [];
            for (let i = 0; i < this.checkedGraph.length; i++) {
                let str = this.checkedGraph[i];

                if (str == 'positive') { graphDataArr.push(this.arrPositive) } else if (str == 'hospitalized') { graphDataArr.push(this.arrhospitalized) } else if (str == 'icu') { graphDataArr.push(this.arrInIcu) } else if (str == 'ventilators') { graphDataArr.push(this.arrOnVentilators) } else if (str == 'recovered') { graphDataArr.push(this.arrRecovered) } else if (str == 'death') { graphDataArr.push(this.arrDeaths) }
            }

            return graphDataArr;
        },
        graphLabel() {
            let graphLabelArr = [];
            for (let i = 0; i < this.checkedGraph.length; i++) {
                let str = this.checkedGraph[i];

                if (str == 'positive') { graphLabelArr.push("Positive") } else if (str == 'hospitalized') { graphLabelArr.push("Hospitalized") } else if (str == 'icu') { graphLabelArr.push("In ICU") } else if (str == 'ventilators') { graphLabelArr.push("On Ventilators") } else if (str == 'recovered') { graphLabelArr.push("Recovered") } else if (str == 'death') { graphLabelArr.push("Deaths") }
            }

            return graphLabelArr;
        },
        graphColors() {
            return this.allGraphColors
        }
    }
}
</script>

<style>
label {
    margin: 20px;
}

#header {
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0px;
    left: 0px;
    right: 0px;
    background: #DEDEDE;
    align-items: center;
    z-index: 1000;
    padding: 20px;
}

#body {
    margin-top: 100px;
}

#nav-links {
    display: flex;
    flex-direction: row;
}

ul {
    list-style: none;
}

#checkboxes {
    display: grid;
    justify-content: center;
    grid-template-columns: auto auto auto auto auto auto;
    grid-template-rows: auto auto auto;
}

#main-chart {
    max-height: 70vh;
    max-width: 70vw;
    display: grid;
    justify-content: center;
    grid-template-columns: auto;
}

#overlays {
    
}
</style>
