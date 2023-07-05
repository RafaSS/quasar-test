<template>
  <div>
    <apexchart
      height="600"
      width="600"
      type="pie"
      :options="options"
      :series="series"
    ></apexchart>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import { getCssVar } from "quasar";

export default defineComponent({
  name: "PopulationByContinent",
  data() {
    return {
      options: {
        title: {
          text: "Podutos por estoque",
          align: "left",
        },
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: 200,
                height: 400,
              },
              title: {
                text: "",
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
        chart: {
          id: "apex-donut",
        },
        colors: [
          getCssVar("secondary"),
          getCssVar("accent"),
          getCssVar("positive"),
          getCssVar("primary"),
          getCssVar("negative"),
          getCssVar("info"),
          getCssVar("warning"),
        ],
        markers: {
          size: 4,
          hover: {
            sizeOffset: 6,
          },
        },
        labels: [],
      },
      series: [],
    };
  },
  beforeMount() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch("https://dummyjson.com/products/")
        .then((res) => res.json())
        .then((data) => {
          const totalStock = data.products.reduce(
            (total, product) => total + product.stock,
            0
          );

          const sortedProducts = data.products.sort(
            (a, b) => b.stock - a.stock
          );

          const limitedProductTitles = sortedProducts
            .slice(0, 6)
            .map((product) => product.title);

          const limitedProductPercentages = sortedProducts
            .slice(0, 6)
            .map((product) => (product.stock / totalStock) * 100);

          const remainingStock = sortedProducts
            .slice(6)
            .reduce((total, product) => total + product.stock, 0);

          this.options = {
            ...this.options,
            labels: [...limitedProductTitles, "Outros"],
          };

          this.series = [
            ...limitedProductPercentages,
            (remainingStock / totalStock) * 100,
          ];
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
  },
});
</script>
