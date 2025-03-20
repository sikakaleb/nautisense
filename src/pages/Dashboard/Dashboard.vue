<template>
  <div class="row">
    <!-- 1) Grande courbe : Variation de la température de l’eau -->
    <div class="col-12">
      <card type="chart">
        <template #header>
          <div class="row">
            <div class="col-sm-6" :class="isRTL ? 'text-right' : 'text-left'">
              <h5 class="card-category">Water Temperature (°C)</h5>
              <h2 class="card-title">Temperature Variation</h2>
            </div>
            <div class="col-sm-6 d-flex d-sm-block">
              <div
                class="btn-group btn-group-toggle"
                :class="isRTL ? 'float-left' : 'float-right'"
                data-toggle="buttons"
              >
                <label
                  v-for="(option, index) in bigLineChartCategories"
                  :key="option.name"
                  class="btn btn-sm btn-primary btn-simple"
                  :class="{ active: bigLineChart.activeIndex === index }"
                  :id="index"
                >
                  <input
                    type="radio"
                    @click="initBigChart(index)"
                    name="options"
                    autocomplete="off"
                    :checked="bigLineChart.activeIndex === index"
                  />
                  <span class="d-none d-sm-block">{{ option.name }}</span>
                  <span class="d-block d-sm-none">
                    <i :class="option.icon"></i>
                  </span>
                </label>
              </div>
            </div>
          </div>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            ref="bigChart"
            :chart-data="bigLineChart.chartData"
            :gradient-colors="bigLineChart.gradientColors"
            :gradient-stops="bigLineChart.gradientStops"
            :extra-options="bigLineChart.extraOptions"
          />
        </div>
      </card>
    </div>
    <div class="col-lg-3 col-md-6" v-for="card in statsCards" :key="card.title">
      <stats-card
        :title="card.title"
        :sub-title="card.subTitle"
        :type="card.type"
        :icon="card.icon"
      >
        <div slot="footer" v-html="card.footer"></div>
      </stats-card>
    </div>
    <!-- 1. CO₂ Emissions -->
    <div class="col-lg-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template #header>
          <h5 class="card-category">CO₂ Emissions</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-cloud-upload-94 text-danger"></i> 763,215
            µg/m³
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            :chart-data="purpleLineChart.chartData"
            :gradient-colors="purpleLineChart.gradientColors"
            :gradient-stops="purpleLineChart.gradientStops"
            :extra-options="purpleLineChart.extraOptions"
          />
        </div>
      </card>
    </div>

    <!-- 2. Salinity -->
    <div class="col-lg-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template #header>
          <h5 class="card-category">Salinity</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-atom text-info"></i> 35 PSU
          </h3>
        </template>
        <div class="chart-area">
          <bar-chart
            style="height: 100%"
            :chart-data="blueBarChart.chartData"
            :gradient-stops="blueBarChart.gradientStops"
            :extra-options="blueBarChart.extraOptions"
          />
        </div>
      </card>
    </div>

    <!-- 3. Water pH -->
    <div class="col-lg-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template #header>
          <h5 class="card-category">Water pH</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-vector text-success"></i> pH = 8.1
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            :chart-data="greenLineChart.chartData"
            :gradient-stops="greenLineChart.gradientStops"
            :extra-options="greenLineChart.extraOptions"
          />
        </div>
      </card>
    </div>

    <!-- 4. Dissolved O₂ -->
    <div class="col-lg-4 mt-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template #header>
          <h5 class="card-category">Dissolved O₂</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-zoom-split text-info"></i> 6.5 mg/L
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            :chart-data="oxygenLineChart.chartData"
            :extra-options="oxygenLineChart.extraOptions"
          />
        </div>
      </card>
    </div>

    <!-- 5. Turbidity -->
    <div class="col-lg-4 mt-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template #header>
          <h5 class="card-category">Turbidity</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-components text-warning"></i> ~12 NTU
          </h3>
        </template>
        <div class="chart-area">
          <bar-chart
            style="height: 100%"
            :chart-data="turbidityChart.chartData"
            :extra-options="turbidityChart.extraOptions"
          />
        </div>
      </card>
    </div>

    <!-- 6. Microplastics -->
    <div class="col-lg-4 mt-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template #header>
          <h5 class="card-category">Microplastics</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-molecule-40 text-danger"></i> 15
            particles/L
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            :chart-data="microplasticsChart.chartData"
            :extra-options="microplasticsChart.extraOptions"
          />
        </div>
      </card>
    </div>

    <div class="col-lg-5">
      <card type="tasks" :header-classes="{ 'text-right': isRTL }">
        <template #header>
          <h6 class="title d-inline">Alerts (5)</h6>
          <p class="card-category d-inline">Today</p>
          <base-dropdown
            menu-on-right=""
            tag="div"
            title-classes="btn btn-link btn-icon"
            :class="{ 'float-left': isRTL }"
          >
            <i slot="title" class="tim-icons icon-settings-gear-63"></i>
            <a class="dropdown-item" href="#pablo">Action</a>
            <a class="dropdown-item" href="#pablo">Another action</a>
            <a class="dropdown-item" href="#pablo">Something else</a>
          </base-dropdown>
        </template>
        <div class="table-full-width table-responsive">
          <task-list></task-list>
        </div>
      </card>
    </div>
    <div class="col-lg-7">
      <card class="card" :header-classes="{ 'text-right': isRTL }">
        <h5 slot="header" class="card-title">Sensors Table</h5>
        <div class="table-responsive">
          <user-table></user-table>
        </div>
      </card>
    </div>
    <div class="col-lg-12">
      <country-map-card></country-map-card>
    </div>
  </div>
</template>

<script>
import LineChart from "@/components/Charts/LineChart";
import BarChart from "@/components/Charts/BarChart";
import * as chartConfigs from "@/components/Charts/config";
import TaskList from "./TaskList";
import UserTable from "./UserTable";
import CountryMapCard from "./CountryMapCard";
import StatsCard from "src/components/Cards/StatsCard";
import config from "@/config";

let bigChartData = [
  // Variation de temp. eau en surface / moyenne / profondeur, par mois
  [15, 17, 19, 20, 22, 25, 24, 22, 21, 20, 18, 16], // Surface
  [14, 15, 16, 17, 18, 19, 19, 18, 18, 17, 16, 15], // Mid-depth
  [10, 10, 11, 12, 12, 13, 14, 13, 12, 11, 10, 10], // Deep
];
let bigChartLabels = [
  "JAN",
  "FEB",
  "MAR",
  "APR",
  "MAY",
  "JUN",
  "JUL",
  "AUG",
  "SEP",
  "OCT",
  "NOV",
  "DEC",
];

let bigChartDatasetOptions = {
  fill: true,
  borderColor: config.colors.primary,
  borderWidth: 2,
  pointBackgroundColor: config.colors.primary,
  pointBorderColor: "rgba(255,255,255,0)",
  pointHoverBackgroundColor: config.colors.primary,
  pointBorderWidth: 20,
  pointHoverRadius: 4,
  pointHoverBorderWidth: 15,
  pointRadius: 4,
};

export default {
  components: {
    LineChart,
    BarChart,
    StatsCard,
    TaskList,
    CountryMapCard,
    UserTable,
  },
  data() {
    return {
      statsCards: [
        {
          title: "24°C",
          subTitle: "Water Temperature",
          type: "warning",
          icon: "tim-icons icon-sound-wave", // Sonde ou mesure de température
          footer: '<i class="tim-icons icon-refresh-01"></i> Updated now',
        },
        {
          title: "45 µg/m³",
          subTitle: "Pollution",
          type: "danger",
          icon: "tim-icons icon-alert-circle-exc", // Pollution ou alerte environnementale
          footer: '<i class="tim-icons icon-sound-wave"></i> Last Research',
        },
        {
          title: "15 knots",
          subTitle: "Vessel Speed",
          type: "info",
          icon: "tim-icons icon-delivery-fast", // Vitesse ou navigation
          footer: '<i class="tim-icons icon-trophy"></i> Sensor feedback',
        },
        {
          title: "8/10",
          subTitle: "Water Quality",
          type: "success",
          icon: "tim-icons icon-planet", // Santé de l'eau
          footer: '<i class="tim-icons icon-watch-time"></i> In the last hours',
        },
        {
          title: "Nutrients",
          subTitle: "Nitrates ~ 4 µmol/L",
          type: "info",
          icon: "tim-icons icon-tap-02", // Mesure d'eau / Nutriments
          footer: '<i class="tim-icons icon-refresh-01"></i> Updated 2h ago',
        },
        {
          title: "Sea Level",
          subTitle: "+3 mm/an",
          type: "warning",
          icon: "tim-icons icon-world", // Niveau de la mer
          footer: '<i class="tim-icons icon-watch-time"></i> Global average',
        },
        {
          title: "Chemical Pol",
          subTitle: "Hg < 1 µg/L",
          type: "danger",
          icon: "tim-icons icon-molecule-40", // Polluant chimique
          footer:
            '<i class="tim-icons icon-settings-gear-63"></i> Last analysis',
        },
        {
          title: "Solar Radiat",
          subTitle: "~ 2.5 W/m²",
          type: "primary",
          icon: "tim-icons icon-sun-cloud", // Radiation solaire
          footer: '<i class="tim-icons icon-sound-wave"></i> For reef areas',
        },
      ],

      // 2) Grand graphique : Variation de T° en surface, mi-profondeur, profonde
      bigLineChart: {
        activeIndex: 0,
        chartData: {
          datasets: [
            {
              ...bigChartDatasetOptions,
              data: bigChartData[0],
            },
          ],
          labels: bigChartLabels,
        },
        extraOptions: chartConfigs.purpleChartOptions,
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.4, 0],
      },

      // 3) Petits graphiques existants
      purpleLineChart: {
        extraOptions: chartConfigs.purpleChartOptions,
        chartData: {
          labels: ["JUL", "AUG", "SEP", "OCT", "NOV", "DEC"],
          datasets: [
            {
              label: "CO₂ Emissions",
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: "rgba(255,255,255,0)",
              pointHoverBackgroundColor: config.colors.primary,
              data: [80, 100, 70, 80, 120, 80],
            },
          ],
        },
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.2, 0],
      },
      greenLineChart: {
        extraOptions: chartConfigs.greenChartOptions,
        chartData: {
          labels: ["JUL", "AUG", "SEP", "OCT", "NOV"],
          datasets: [
            {
              label: "Water pH",
              fill: true,
              borderColor: config.colors.danger,
              borderWidth: 2,
              pointBackgroundColor: config.colors.danger,
              pointHoverBackgroundColor: config.colors.danger,
              data: [8.2, 8.0, 7.9, 8.1, 8.1],
            },
          ],
        },
        gradientColors: [
          "rgba(66,134,121,0.15)",
          "rgba(66,134,121,0.0)",
          "rgba(66,134,121,0)",
        ],
        gradientStops: [1, 0.4, 0],
      },
      blueBarChart: {
        extraOptions: chartConfigs.barChartOptions,
        chartData: {
          labels: ["JAN", "FEB", "MAR", "APR", "MAY", "JUN"],
          datasets: [
            {
              label: "Salinity (PSU)",
              fill: true,
              borderColor: config.colors.info,
              borderWidth: 2,
              data: [34, 35, 35, 36, 35, 34],
            },
          ],
        },
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.4, 0],
      },

      // 4) Nouveaux petits graphiques : O₂, Turbidity, Microplastics
      oxygenLineChart: {
        extraOptions: chartConfigs.lineChartOptionsBlue,
        chartData: {
          labels: ["JUL", "AUG", "SEP", "OCT", "NOV"],
          datasets: [
            {
              label: "Dissolved O₂ (mg/L)",
              borderColor: config.colors.info,
              fill: false,
              data: [6, 6.5, 6.8, 6.2, 6.4],
            },
          ],
        },
      },
      turbidityChart: {
        extraOptions: chartConfigs.barChartOptions,
        chartData: {
          labels: ["JAN", "FEB", "MAR", "APR", "MAY"],
          datasets: [
            {
              label: "NTU",
              backgroundColor: config.colors.warning,
              data: [10, 12, 9, 15, 13],
            },
          ],
        },
      },
      microplasticsChart: {
        extraOptions: chartConfigs.lineChartOptionsBlue,
        chartData: {
          labels: ["Q1", "Q2", "Q3", "Q4"],
          datasets: [
            {
              label: "Particles/L",
              borderColor: config.colors.danger,
              fill: false,
              data: [12, 15, 13, 18],
            },
          ],
        },
      },
    };
  },
  computed: {
    // RTL Handling (si besoin)
    enableRTL() {
      return this.$route.query.enableRTL;
    },
    isRTL() {
      return this.$rtl.isRTL;
    },
    // Boutons d'options pour le grand graphique (Surface, Mid-depth, Deep water)
    bigLineChartCategories() {
      return [
        { name: "Surface", icon: "tim-icons icon-minimal-left" }, 
        { name: "Mid-depth", icon: "tim-icons icon-minimal-down" }, // 
        { name: "Deep water", icon: "tim-icons icon-minimal-right" }, 
      ];
    },
  },
  methods: {
    // Mise à jour du grand graphique de température en fonction de l'index
    initBigChart(index) {
      let chartData = {
        datasets: [
          {
            ...bigChartDatasetOptions,
            data: bigChartData[index],
          },
        ],
        labels: bigChartLabels,
      };
      // Met à jour le gradient sur le chart
      this.$refs.bigChart.updateGradients(chartData);
      this.bigLineChart.chartData = chartData;
      this.bigLineChart.activeIndex = index;
    },
  },
  mounted() {
    // Gestion éventuelle de la langue ou du RTL
    this.i18n = this.$i18n;
    if (this.enableRTL) {
      this.i18n.locale = "ar";
      this.$rtl.enableRTL();
    }
    // Initialise le grand graphique sur l'index 0
    this.initBigChart(0);
  },
  beforeDestroy() {
    // On repasse en mode LTR si on s'en va
    if (this.$rtl.isRTL) {
      this.i18n.locale = "en";
      this.$rtl.disableRTL();
    }
  },
};
</script>

<style scoped>
/* Vous pouvez ajouter des ajustements de style ici si nécessaire */

.mt-4 {
  margin-top: 1.5rem !important;
}
</style>

<style></style>
