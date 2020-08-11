<script>
import { Line } from "vue-chartjs";

export default {
    extends: Line,
    props: {
        labels: {
            type: Array
        },
        chartData: {
            type: Array
        },
        chartColors: {
            type: Array
        },
        options: {
            type: Object
        }
    },
    mounted() {

        let totals = null;

        let datasets = [];
        for (let i = 0; i < this.chartData.length; i++) {

            totals = this.chartData[i].map(d => d.total).reverse();
            let { borderColor, pointDorderColor, pointBackgroundColor, backgroundColor } = this.chartColors[i];

            datasets.push({
                fill: true,
                label: this.labels[i],
                data: totals,
                borderColor: borderColor, // color of line
                pointDorderColor: pointDorderColor, // color of circle
                pointBackgroundColor: pointBackgroundColor, // background color of point
                backgroundColor: backgroundColor // color of graph
            })
        }

        const dates = this.chartData[0].map(d => d.date).reverse();

        this.renderChart({
                labels: dates,
                datasets: datasets
            },
            this.options
        );
    }
}
/* 
 let totals = null;
       

        const dates = this.chartData[0].map(d => d.date).reverse();
        for(let i = 0; i < this.chartData.length; i++){
            console.log(i);
            let { borderColor, pointDorderColor, pointBackgroundColor, backgroundColor } = this.chartColors[i];
            totals = this.chartData[i].map(d => d.total).reverse();
            this.renderChart({
                labels: dates,
                datasets: [{
                    label: this.labels[i],
                    data: totals,
                    borderColor: borderColor, // color of line
                    pointDorderColor: pointDorderColor, // color of circle
                    pointBackgroundColor: pointBackgroundColor, // background color of point
                    backgroundColor: backgroundColor // color of graph
                }]
            }, 
            this.options
            );
        } */
</script>

