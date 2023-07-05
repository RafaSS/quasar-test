<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
          </q-avatar>
          Title
        </q-toolbar-title>

        <q-btn dense flat round icon="menu" @click="toggleRightDrawer" />
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="leftDrawerOpen" side="left" bordered>
      <q-scroll-area class="fit">
        <q-list>
          <template v-for="(menuItem, index) in menuList" :key="index">
            <q-item clickable :active="menuItem.label === 'Outbox'" v-ripple>
              <q-item-section avatar>
                <q-icon :name="menuItem.icon" />
              </q-item-section>
              <q-item-section>
                {{ menuItem.label }}
              </q-item-section>
            </q-item>
            <q-separator :key="'sep' + index" v-if="menuItem.separator" />
          </template>
        </q-list>
      </q-scroll-area>
    </q-drawer>

    <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <q-toolbar class="q-mt-lg">
        <q-space></q-space>
        <q-btn round icon="notifications">
          <q-badge floating color="green" rounded />
        </q-btn>
      </q-toolbar>
      <div class="flex flex-center column">
        <q-avatar size="100px">
          <img src="https://cdn.quasar.dev/img/avatar6.jpg" />
        </q-avatar>
        <q-item-label caption class="q-my-md">Your Activity Today</q-item-label>
        <div class="text-h5"><strong>86%</strong></div>
      </div>
      <q-card class="q-mx-md" flat bordered>
        <q-toolbar class="">
          <div class="text-overline">All activity</div>
          <q-space></q-space>
          <q-btn
            round
            icon="fas fa-chevron-right"
            size="10px"
            color="green"
          ></q-btn>
        </q-toolbar>
      </q-card>
    </q-drawer>

    <q-page-container>
      <div class="q-pa-md">
        <q-toolbar>
          <h6 class="text-weight-bolder">Gráficos</h6>
          <q-space></q-space>
          <q-icon name="fas fa-calendar-minus" size="" class="q-mr-sm" />
        </q-toolbar>
        <div class="row q-col-gutter-md top">
          <div class="col-12 col-lg-6 flex">
            <q-card class="q-mx-md" bordered>
              <div class="flex flex-center">
                <PieChart />
              </div>
            </q-card>
          </div>
          <div class="col-12 col-lg-6">
            <q-card class="q-mx-md" bordered>
              <div class="flex-center">
                <ColumnChart />
              </div>
            </q-card>
          </div>
          <div class="col-12 col-lg-12">
            <q-card class="q-mx-md" bordered>
              <div class="flex-center">
                <BarChart />
              </div>
            </q-card>
          </div>
        </div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref, defineAsyncComponent, onMounted } from "vue";
import { Notify } from "quasar";
const PieChart = defineAsyncComponent(() =>
  import("src/components/charts/PieChart.vue")
);
const BarChart = defineAsyncComponent(() =>
  import("src/components/charts/BarChart.vue")
);
const ColumnChart = defineAsyncComponent(() =>
  import("src/components/charts/ColumnChart.vue")
);
const menuList = [
  {
    icon: "inbox",
    label: "Inbox",
    separator: true,
  },
  {
    icon: "send",
    label: "Outbox",
    separator: false,
  },
  {
    icon: "delete",
    label: "Trash",
    separator: false,
  },
  {
    icon: "error",
    label: "Spam",
    separator: true,
  },
  {
    icon: "settings",
    label: "Settings",
    separator: false,
  },
  {
    icon: "feedback",
    label: "Send Feedback",
    separator: false,
  },
  {
    icon: "help",
    iconColor: "primary",
    label: "Help",
    separator: false,
  },
];

export default defineComponent({
  name: "MainLayout",

  components: {
    PieChart,
    BarChart,
    ColumnChart,
  },

  setup() {
    const leftDrawerOpen = ref(false);
    const rightDrawerOpen = ref(false);

    onMounted(() => {
      Notify.create({
        message: "Bem-vindo(a) ao website de demonstração de dashboard!",
        color: "positive",
        multiLine: true,
        position: "center",
        actions: [
          {
            label: "Ok!",
            color: "yellow",
            handler: () => {
              /* ... */
            },
          },
        ],
      });
    });

    return {
      leftDrawerOpen,
      rightDrawerOpen,
      menuList,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      toggleRightDrawer() {
        rightDrawerOpen.value = !rightDrawerOpen.value;
      },
    };
  },
});
</script>

<style scoped>
.search {
  width: 700px;
}
.doctors {
  height: 220px;
}
.overlapping {
  border: 2px solid white;
  position: absolute;
  margin-left: -20px;
}
.card1 {
  height: 100px;
}
.top {
  margin-top: -40px;
}
.colorCard {
  background: #f8f8f8;
}
</style>
