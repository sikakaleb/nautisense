<template>
  <div class="row">
    <div class="col-12">
      <card type="chart">
        <template slot="header">
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
          >
          </line-chart>
        </div>
      </card>
    </div>

    <!-- Stats Cards (exemple d'indicateurs clés) -->
    <div
      class="col-lg-3 col-md-6"
      v-for="card in statsCards"
      :key="card.title"
    >
      <stats-card
        :title="card.title"
        :sub-title="card.subTitle"
        :type="card.type"
        :icon="card.icon"
      >
        <div slot="footer" v-html="card.footer"></div>
      </stats-card>
    </div>

    <!-- Small charts (3 colonnes) -->
    <div class="col-lg-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template slot="header">
          <h5 class="card-category">CO₂ Emissions</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-bell-55 text-primary"></i> 763,215 µg/m³
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            :chart-data="purpleLineChart.chartData"
            :gradient-colors="purpleLineChart.gradientColors"
            :gradient-stops="purpleLineChart.gradientStops"
            :extra-options="purpleLineChart.extraOptions"
          >
          </line-chart>
        </div>
      </card>
    </div>

    <div class="col-lg-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template slot="header">
          <h5 class="card-category">Salinity</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-delivery-fast text-info"></i> 35 PSU
          </h3>
        </template>
        <div class="chart-area">
          <bar-chart
            style="height: 100%"
            :chart-data="blueBarChart.chartData"
            :gradient-stops="blueBarChart.gradientStops"
            :extra-options="blueBarChart.extraOptions"
          >
          </bar-chart>
        </div>
      </card>
    </div>

    <div class="col-lg-4" :class="{ 'text-right': isRTL }">
      <card type="chart">
        <template slot="header">
          <h5 class="card-category">Water pH</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-send text-success"></i> pH = 8.1
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
            :chart-data="greenLineChart.chartData"
            :gradient-stops="greenLineChart.gradientStops"
            :extra-options="greenLineChart.extraOptions"
          >
          </line-chart>
        </div>
      </card>
    </div>

    <!-- "Tasks" (renommé ou conservé) -->
    <div class="col-lg-5">
      <card type="tasks" :header-classes="{ 'text-right': isRTL }">
        <template slot="header">
          <!-- Par exemple, on peut renommer Tasks en Alerts -->
          <h6 class="title d-inline">Alerts (5)</h6>
          <p class="card-category d-inline">Today</p>
          <base-dropdown
            menu-on-right=""
            tag="div"
            title-classes="btn btn-link btn-icon"
            :class="{ 'float-left': isRTL }"
          >
            <i slot="title" class="tim-icons icon-settings-gear-63"></i>
            <a class="dropdown-item" href="#pablo"> Action </a>
            <a class="dropdown-item" href="#pablo"> Another action </a>
            <a class="dropdown-item" href="#pablo"> Something else </a>
          </base-dropdown>
        </template>
        <div class="table-full-width table-responsive">
          <task-list></task-list>
        </div>
      </card>
    </div>

    <!-- Tableau de management (exemple : table capteurs) -->
    <div class="col-lg-7">
      <card class="card" :header-classes="{ 'text-right': isRTL }">
        <h5 slot="header" class="card-title">Sensors Table</h5>
        <div class="table-responsive">
          <user-table></user-table>
        </div>
      </card>
    </div>

    <!-- Carte (par exemple, on garde le composant existant) -->
    <div class="col-lg-12">
      <country-map-card></country-map-card>
    </div>
  </div>
</template>

<script>
import LineChart from '@/components/Charts/LineChart';
import BarChart from '@/components/Charts/BarChart';
import * as chartConfigs from '@/components/Charts/config';
import TaskList from './TaskList';
import UserTable from './UserTable';
import CountryMapCard from './CountryMapCard';
import StatsCard from 'src/components/Cards/StatsCard';
import config from '@/config';

let bigChartData = [
  [15, 17, 19, 20, 22, 25, 24, 22, 21, 20, 18, 16],
  [14, 15, 16, 17, 18, 19, 19, 18, 18, 17, 16, 15], 
  [10, 10, 11, 12, 12, 13, 14, 13, 12, 11, 10, 10]  
];
let bigChartLabels = ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'];

let bigChartDatasetOptions = {
  fill: true,
  borderColor: config.colors.primary,
  borderWidth: 2,
  borderDash: [],
  borderDashOffset: 0.0,
  pointBackgroundColor: config.colors.primary,
  pointBorderColor: 'rgba(255,255,255,0)',
  pointHoverBackgroundColor: config.colors.primary,
  pointBorderWidth: 20,
  pointHoverRadius: 4,
  pointHoverBorderWidth: 15,
  pointRadius: 4
};

export default {
  components: {
    LineChart,
    BarChart,
    StatsCard,
    TaskList,
    CountryMapCard,
    UserTable
  },
  data() {
    return {
      statsCards: [
        {
          title: '24°C',
          subTitle: 'Temp. Eau',
          type: 'warning',
          icon: 'tim-icons icon-chat-33',
          footer: '<i class="tim-icons icon-refresh-01"></i> Update Now'
        },
        {
          title: '45 µg/m³',
          subTitle: 'Pollution',
          type: 'primary',
          icon: 'tim-icons icon-shape-star',
          footer: '<i class="tim-icons icon-sound-wave"></i> Last Research'
        },
        {
          title: '15 nœuds',
          subTitle: 'Vitesse',
          type: 'info',
          icon: 'tim-icons icon-single-02',
          footer: '<i class="tim-icons icon-trophy"></i> Sensor feedback'
        },
        {
          title: '8/10',
          subTitle: 'Qualité',
          type: 'danger',
          icon: 'tim-icons icon-molecule-40',
          footer: '<i class="tim-icons icon-watch-time"></i> In the last hours'
        }
      ],
      bigLineChart: {
        activeIndex: 0,
        chartData: {
          datasets: [
            {
              ...bigChartDatasetOptions,
              data: bigChartData[0]
            }
          ],
          labels: bigChartLabels
        },
        extraOptions: chartConfigs.purpleChartOptions,
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.4, 0]
      },
      purpleLineChart: {
        extraOptions: chartConfigs.purpleChartOptions,
        chartData: {
          labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'],
          datasets: [
            {
              label: 'CO₂ Emissions',
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary,
              data: [80, 100, 70, 80, 120, 80]
            }
          ]
        },
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.2, 0]
      },
      greenLineChart: {
        extraOptions: chartConfigs.greenChartOptions,
        chartData: {
          labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV'],
          datasets: [
            {
              label: 'Water pH',
              fill: true,
              borderColor: config.colors.danger,
              borderWidth: 2,
              pointBackgroundColor: config.colors.danger,
              pointHoverBackgroundColor: config.colors.danger,
              data: [8.2, 8.0, 7.9, 8.1, 8.1]
            }
          ]
        },
        gradientColors: [
          'rgba(66,134,121,0.15)',
          'rgba(66,134,121,0.0)',
          'rgba(66,134,121,0)'
        ],
        gradientStops: [1, 0.4, 0]
      },
      blueBarChart: {
        extraOptions: chartConfigs.barChartOptions,
        chartData: {
          labels: ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN'],
          datasets: [
            {
              label: 'Salinity (PSU)',
              fill: true,
              borderColor: config.colors.info,
              borderWidth: 2,
              data: [34, 35, 35, 36, 35, 34]
            }
          ]
        },
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.4, 0]
      }
    };
  },
  computed: {
    enableRTL() {
      return this.$route.query.enableRTL;
    },
    isRTL() {
      return this.$rtl.isRTL;
    },

    bigLineChartCategories() {
      return [
        { name: 'Surface', icon: 'tim-icons icon-single-02' },
        { name: 'Mid-depth', icon: 'tim-icons icon-gift-2' },
        { name: 'Deep water', icon: 'tim-icons icon-tap-02' }
      ];
    }
  },
  methods: {

    initBigChart(index) {
      let chartData = {
        datasets: [
          {
            ...bigChartDatasetOptions,
            data: bigChartData[index]
          }
        ],
        labels: bigChartLabels
      };
      this.$refs.bigChart.updateGradients(chartData);
      this.bigLineChart.chartData = chartData;
      this.bigLineChart.activeIndex = index;
    }
  },
  mounted() {
    this.i18n = this.$i18n;
    if (this.enableRTL) {
      this.i18n.locale = 'ar';
      this.$rtl.enableRTL();
    }

    this.initBigChart(0);
  },
  beforeDestroy() {
    if (this.$rtl.isRTL) {
      this.i18n.locale = 'en';
      this.$rtl.disableRTL();
    }
  }
};
</script>
<style></style>
