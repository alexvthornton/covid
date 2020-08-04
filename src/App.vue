<template>
    <div id="app">
        <div id="header" class="col text-center">
    
            <div class="col text-center">
                <h1>COVID-19 Data Visualization</h1>
    
            </div>
            <nav id="nav-links" class="text-center">
                <a class="nav-link" href="#positive">Positive</a>
                <a class="nav-link" href="#hospitalized">Hospitalized</a>
                <a class="nav-link" href="#icu">In ICU</a>
                <a class="nav-link" href="#ventilators">On Ventilators</a>
                <a class="nav-link" href="#recovered">Recovered</a>
                <a class="nav-link" href="#deaths">Deaths</a>
                <a class="nav-link" href="#overlays">Overlays</a>
            </nav>
    
        </div>
    
        <body id="body" class="container">
            <div></div>
            <div id="positive" class="row mt-5" v-if="arrPositive.length > 0">
                <div class="col">
                    <h2>Positive</h2>
                    <line-chart :chartData="arrPositive" :options="chartOptions" label="Positive" :chartColors="positiveChartColors"></line-chart>
                </div>
            </div>
    
            <div id="hospitalized" class="row mt-5" v-if="arrhospitalized.length > 0">
                <div class="col">
                    <h2>Hospitalized</h2>
                    <line-chart :chartData="arrhospitalized" :options="chartOptions" label="Hospitalized" :chartColors="hospitalizedChartColors"></line-chart>
                </div>
            </div>
    
            <div id="icu" class="row mt-5" v-if="arrInIcu.length > 0">
                <div class="col">
                    <h2>In ICU</h2>
                    <line-chart :chartData="arrInIcu" :options="chartOptions" label="In ICU" :chartColors="ICUChartColors"></line-chart>
                </div>
            </div>
    
            <div id="ventilators" class="row mt-5" v-if="arrOnVentilators.length > 0">
                <div class="col">
                    <h2>On Ventilators</h2>
                    <line-chart :chartData="arrOnVentilators" :options="chartOptions" label="On Ventilators" :chartColors="ventilatorChartColors"></line-chart>
                </div>
            </div>
    
            <div id="recovered" class="row mt-5" v-if="arrRecovered.length > 0">
                <div class="col">
                    <h2>Recovered</h2>
                    <line-chart :chartData="arrRecovered" :options="chartOptions" label="Recovered" :chartColors="recoveredChartColors"></line-chart>
                </div>
            </div>
    
            <div id="deaths" class="row mt-5" v-if="arrDeaths.length > 0">
                <div class="col">
                    <h2>Deaths</h2>
                    <line-chart :chartData="arrDeaths" :options="chartOptions" label="Deaths" :chartColors="deathChartColors"></line-chart>
                </div>
            </div>
    
    
            <div id="overlays" class="row mt-5" v-if="arrDeaths.length > 0">
                <div class="col">
                    <h2>Overlays</h2>
                    <section class="col text-center" id="checkbox-wrapper">
                        <label for="positive">
                                    <input name="graphs" value="positive" type="checkbox" @click="togglePositive">
                                    Positive
                                </label>
                        <label for="hospitalized">
                                    <input name="graphs" value="hospitalized" type="checkbox" @click="toggleHospitalized" checked>
                                    Hospitalized
                                </label>
                        <label for="in-ICU">
                                    <input name="graphs" value="in-ICU" type="checkbox" @click="toggleICU">
                                    In ICU
                                </label>
                        <label for="on-Ventilators">
                                    <input name="graphs" value="on-Ventilators" type="checkbox"  @click="toggleVentilators">
                                    On Ventilators
                                </label>
                        <label for="deaths">
                                    <input name="graphs" value="deaths" type="checkbox" @click="toggleDeaths" checked>
                                    Deaths
                                </label>
    
                    </section>
                    <multi-data-line-chart :chartDataOne="arrDeaths" :chartDataTwo="arrhospitalized" :options="chartOptions" labelOne="Deaths" labelTwo="Hospitalized" :chartColorsOne="chartOneColors" :chartColorsTwo="chartTwoColors"></multi-data-line-chart>
                </div>
            </div>
        </body>
    </div>
</template>

<script>
import axios from 'axios'; // what we will use to make calls to API
import moment from 'moment'; // for date formating

import LineChart from "./components/lineChart"
import multiDataLineChart from "./components/multiDataLineChart"

export default {
    name: 'App',
    components: {
        LineChart,
        multiDataLineChart
    },
    data() {
        return {
            arrPositive: [],
            positiveChecked: false,
            positiveChartColors: {
                borderColor: "#077187",
                pointDorderColor: "#0E1428",
                pointBackgroundColor: "#AFD6AC",
                backgroundColor: "rgba(44,6,78,0.5)"
            },
            arrhospitalized: [],
            hospitalizedChecked: true,
            hospitalizedChartColors: {
                borderColor: "#077187",
                pointDorderColor: "#0E1428",
                pointBackgroundColor: "#AFD6AC",
                backgroundColor: "#74A57F"
            },
            arrInIcu: [],
            inICUChecked: false,
            ICUChartColors: {
                borderColor: "#077187",
                pointDorderColor: "#0E1428",
                pointBackgroundColor: "#AFD6AC",
                backgroundColor: "#74A57F"
            },
            arrOnVentilators: [],
            onVentilatorsChecked: false,
            ventilatorChartColors: {
                borderColor: "#077187",
                pointDorderColor: "#0E1428",
                pointBackgroundColor: "#AFD6AC",
                backgroundColor: "#74A57F"
            },
            arrRecovered: [],
            recoveredChecked: false,
            recoveredChartColors: {
                borderColor: "#077187",
                pointDorderColor: "#0E1428",
                pointBackgroundColor: "#AFD6AC",
                backgroundColor: "#74A57F"
            },
            arrDeaths: [],
            deathsChecked: true,
            deathChartColors: {
                borderColor: "#077187",
                pointDorderColor: "#0E1428",
                pointBackgroundColor: "#AFD6AC",
                backgroundColor: "#74A57F"
            },
            chartOptions: {
                responsive: true,
                maintainAspectRation: false
            },
            chartOneColors: {
                borderColorOne: "#077187",
                pointDorderColorOne: "#0E1428",
                pointBackgroundColorOne: "#AFD6AC",
                backgroundColorOne: "#0716B860"
            },
            chartTwoColors: {
                borderColorTwo: "#381A1670",
                pointDorderColorTwo: "#470D06B0",
                pointBackgroundColorTwo: "#D05E51B0",
                backgroundColorTwo: "#941B0DB0"
            },
            checkedGraphs: [],

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

            console.log(this.arrPositive);
        })
    },
    methods: {
        togglePositive() {
            this.positiveChecked = !this.positiveChecked
        },
        toggleHospitalized() {
            this.hospitalizedChecked = !this.hospitalizedChecked
        },
        toggleICU() {
            this.inICUChecked = !this.inICUChecked
        },
        toggleVentilators() {
            this.onVentilatorsChecked = !this.onVentilatorsChecked
        },
        toggleDeaths() {
            this.deathsChecked = !this.deathsChecked
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
    z-index: 1000
}

#body {
    margin-top: 150px;
}

#nav-links {
    display: flex;
    flex-direction: row;
}

</style>
