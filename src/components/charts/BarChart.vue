<template>
  <apexchart
    height="100"
    type="bar"
    :options="options"
    :series="series"
  ></apexchart>
</template>

<script>
import { defineComponent } from "vue";
import { getCssVar } from "quasar";

export default defineComponent({
  name: "ApexBar",
  data() {
    return {
      options: {
        title: {
          text: "ApexBar",
          align: "left",
        },
        chart: {
          id: "apex-bar",
          type: "bar",
          height: 350,
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
            horizontal: true,
            columnWidth: "55%",
            endingShape: "rounded",
          },
        },
      },
      series: {},
    };
  },
  beforeMount() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch("https://dummyjson.com/products")
        .then((res) => res.json())
        .then((data) => {
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

          this.options = {
            ...this.options,
            xaxis: {
              categories: limitedProductTitles,
            },
          };

          this.series = [
            {
              name: "series-1",
              data: limitedProductStocks,
            },
          ];

          console.log(this.options.xaxis.categories);
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
  },
});
</script>
