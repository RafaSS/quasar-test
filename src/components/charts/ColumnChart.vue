<template>
  <apexchart type="bar" :options="options" :series="series"></apexchart>
</template>

<script>
import { defineComponent } from "vue";
import { getCssVar } from "quasar";

export default defineComponent({
  name: "ApexColumn",
  data() {
    return {
      options: {
        title: {
          text: "ApexColumn",
          align: "left",
        },
        chart: {
          id: "apex-column",
        },
        colors: [
          getCssVar("primary"),
          getCssVar("secondary"),
          getCssVar("negative"),
        ],
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: 200,
              },
              legend: {
                position: "bottom",
              },
            },
          },
        ],
        plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: "55%",
            endingShape: "rounded",
          },
        },
      },
      series: [],
    };
  },
  beforeMount() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      // Fetch data from the API
      fetch("https://dummyjson.com/products")
        .then((res) => res.json())
        .then((data) => {
          // Sort the products based on stock in descending order
          const sortedProducts = data.products.sort(
            (a, b) => b.stock - a.stock
          );

          // Extract the limited product titles and stocks
          const limitedProductTitles = sortedProducts
            .slice(0, 6)
            .map((product) => product.title);
          const limitedProductStocks = sortedProducts
            .slice(0, 6)
            .map((product) => product.stock);

          // Update the options categories with the limited product titles
          this.options = {
            ...this.options,
            xaxis: {
              categories: limitedProductTitles,
            },
          };

          // Update the series data with the limited product stocks
          this.series = [
            {
              name: "series-1",
              data: limitedProductStocks,
            },
          ];
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
  },
});
</script>
