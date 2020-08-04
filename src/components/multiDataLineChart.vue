<script>
import { Line } from "vue-chartjs";

export default {
    extends: Line,
    props: {
        labelOne: {
            type: String
        },
        labelTwo: {
            type: String
        },
        chartDataOne: {
            type: Array
        },
        chartDataTwo: {
            type: Array
        },
        options: {
            type: Object
        },
        chartColorsOne: {
            type: Object
        },
        chartColorsTwo: {
            type: Object
        }
    },
    mounted() {
        const dates = this.chartDataOne.map(d => d.date).reverse();

        const totalsOne = this.chartDataOne.map(d => d.total).reverse();
        const totalsTwo = this.chartDataTwo.map(d => d.total).reverse();

        const { borderColorOne, pointDorderColorOne, pointBackgroundColorOne, backgroundColorOne } = this.chartColorsOne
        const { borderColorTwo, pointDorderColorTwo, pointBackgroundColorTwo, backgroundColorTwo } = this.chartColorsTwo
        this.renderChart({
                labels: dates,
                datasets: [{
                    label: this.labelOne,
                    data: totalsOne,
                    borderColor: borderColorOne, // color of line
                    pointDorderColor: pointDorderColorOne, // color of circle
                    pointBackgroundColor: pointBackgroundColorOne, // background color of point
                    backgroundColor: backgroundColorOne // color of graph
                }, {
                    label: this.labelTwo,
                    data: totalsTwo,
                    borderColor: borderColorTwo, // color of line
                    pointDorderColor: pointDorderColorTwo, // color of circle
                    pointBackgroundColor: pointBackgroundColorTwo, // background color of point
                    backgroundColor: backgroundColorTwo // color of graph

                }]
            },
            this.options
        );
    }
}
</script>
