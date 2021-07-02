<script>
import { Line } from "vue3-chart-v2";

export default {
  extends: Line,
  props: ['country'],
  data() {
    return {
      dataCases: {
            label: "Data One",
            borderColor: "#FC2525",
            pointBackgroundColor: "white",
            borderWidth: 1,
            pointBorderColor: "white",
            backgroundColor: null,
            data: [40, 39, 10, 40, 39, 80, 40]
      },
      dataDeaths: {
            label: "Data Two",
            borderColor: "#05CBE1",
            pointBackgroundColor: "white",
            pointBorderColor: "white",
            borderWidth: 1,
            backgroundColor: null,
            data: [60, 55, 32, 10, 2, 12, 53]
      }, 
      dataRecovers: {
            label: "Data Three",
            borderColor: "yellow",
            pointBackgroundColor: "white",
            borderWidth: 1,
            pointBorderColor: "white",
            backgroundColor: null,
            data: [10, 20, 3, 80, 12, 10, 10]
      }
    };
  },
  methods: {
      getCountryData: async (countryName) => {
          const res = await fetch('https://api.covid19api.com/dayone/country/' +  encodeURI(countryName));
          console.log(res);
      }
  },
  mounted() {

    //   getCountryData(this.country);


    this.dataCases.backgroundColor = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);
    this.dataDeaths.backgroundColor = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);
    this.dataRecovers.backgroundColor = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);

    this.dataCases.backgroundColor.addColorStop(0, "rgba(255, 0,0, 0.5)");
    this.dataCases.backgroundColor.addColorStop(0.5, "rgba(255, 0, 0, 0.25)");
    this.dataCases.backgroundColor.addColorStop(1, "rgba(255, 0, 0, 0)");

    this.dataDeaths.backgroundColor.addColorStop(0, "rgba(0, 231, 255, 0.9)");
    this.dataDeaths.backgroundColor.addColorStop(0.5, "rgba(0, 231, 255, 0.25)");
    this.dataDeaths.backgroundColor.addColorStop(1, "rgba(0, 231, 255, 0)");

    this.dataRecovers.backgroundColor.addColorStop(0, "rgba(255, 249, 65, 0.9)");
    this.dataRecovers.backgroundColor.addColorStop(0.5, "rgba(255, 249, 65, 0.5)");
    this.dataRecovers.backgroundColor.addColorStop(1, "rgba(255, 249, 65, 0)");

    // this.dataCases.backgroundColor = this.gradient;
    

    this.renderChart(
      {
        labels: [
          "January",
          "February",
          "March",
          "April",
          "May",
          "June",
          "July"
        ],
        datasets: [
          this.dataDeaths, this.dataCases, this.dataRecovers
        ]
      },
      { responsive: true, maintainAspectRatio: false }
    );
  }
};
</script>
