<script>
import { Line } from "vue-chartjs";

export default {
    extends: Line,
    props: {
        label: {
            type: String
        },
        chartData: {
            type: Array
        },
        options: {
            type: Object
        },
        chartColors: {
            type: Object
        }
    },
    mounted() {
        const dates = this.chartData.map(d => d.date).reverse();
        const totals = this.chartData.map(d => d.total).reverse();

        const {borderColor, pointDorderColor, pointBackgroundColor, backgroundColor } = this.chartColors
        this.renderChart({
                labels: dates,
                datasets: [{
                    label: this.label,
                    data: totals,
                    borderColor: borderColor,                    // color of line
                    pointDorderColor: pointDorderColor,          // color of circle
                    pointBackgroundColor: pointBackgroundColor,  // background color of point
                    backgroundColor: backgroundColor             // color of graph
                }]
            },
            this.options
        );
    }
}
</script>
