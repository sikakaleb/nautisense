<template>
  <div>
    <!-- Header Section -->
    <div class="text-center mb-4">
      <h2>Global Nautisense Dashboard</h2>
      <p class="category">
        Monitor worldwide revenues, user activities, operational costs, and general performance.
      </p>
    </div>

    <!-- Financial Overview -->
    <div class="row mt-5">
      <div class="col-md-4">
        <card class="card-chart" no-footer-line>
          <template slot="header">
            <h5 class="card-category">Revenue</h5>
            <h3 class="card-title">
              <i class="tim-icons icon-coins text-success"></i> $1,200,000
            </h3>
          </template>
          <div class="chart-area">
            <line-chart
              :chart-data="revenueChart.chartData"
              :extra-options="revenueChart.extraOptions"
              :height="200"
            ></line-chart>
          </div>
        </card>
      </div>

      <div class="col-md-4">
        <card class="card-chart">
          <template slot="header">
            <h5 class="card-category">Expenses</h5>
            <h3 class="card-title">
              <i class="tim-icons icon-wallet-43 text-danger"></i> $850,000
            </h3>
          </template>
          <div class="chart-area">
            <bar-chart
              :chart-data="expensesChart.chartData"
              :extra-options="expensesChart.extraOptions"
              :height="200"
            ></bar-chart>
          </div>
        </card>
      </div>

      <div class="col-md-4">
        <card class="card-chart">
          <template slot="header">
            <h5 class="card-category">Net Profit</h5>
            <h3 class="card-title">
              <i class="tim-icons icon-chart-bar-32 text-info"></i> $350,000
            </h3>
          </template>
          <div class="chart-area">
            <line-chart
              :chart-data="profitChart.chartData"
              :extra-options="profitChart.extraOptions"
              :height="200"
            ></line-chart>
          </div>
        </card>
      </div>
    </div>

    <!-- User Activity Analysis -->
    <div class="row mt-5">
      <div class="col-md-6">
        <card class="card-chart card-chart-pie">
          <h5 slot="header" class="card-category">User Distribution</h5>
          <div class="row">
            <div class="col-6">
              <div class="chart-area">
                <pie-chart
                  :chart-data="userChart.chartData"
                  :extra-options="userChart.extraOptions"
                  :height="150"
                ></pie-chart>
              </div>
            </div>
            <div class="col-6">
              <h4 class="card-title">
                <i class="tim-icons icon-single-02 text-primary"></i> 200K Active Users
              </h4>
              <p class="category">User engagement over the past month</p>
            </div>
          </div>
        </card>
      </div>

      <div class="col-md-6">
        <card class="card-chart">
          <template slot="header">
            <h5 class="card-category">Market Share</h5>
            <h3 class="card-title">
              <i class="tim-icons icon-world text-warning"></i> 80% Global
            </h3>
          </template>
          <div class="chart-area">
            <pie-chart
              :chart-data="marketShareChart.chartData"
              :extra-options="marketShareChart.extraOptions"
              :height="200"
            ></pie-chart>
          </div>
        </card>
      </div>
    </div>
  </div>
</template>


<script>
import LineChart from 'src/components/Charts/LineChart';
import BarChart from 'src/components/Charts/BarChart';
import PieChart from 'src/components/Charts/PieChart';
import config from '@/config';
import * as chartConfigs from '@/components/Charts/config';

export default {
  components: {
    LineChart,
    BarChart,
    PieChart
  },
  data() {
    return {
      revenueChart: {
        chartData: {
          labels: ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN'],
          datasets: [{ label: 'Revenue ($)', data: [200000, 250000, 180000, 220000, 200000, 150000] }]
        },
        extraOptions: chartConfigs.lineChartOptionsBlue
      },
      expensesChart: {
        chartData: {
          labels: ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN'],
          datasets: [{ label: 'Expenses ($)', data: [150000, 180000, 170000, 190000, 160000, 100000] }]
        },
        extraOptions: chartConfigs.barChartOptionsGradient
      },
      profitChart: {
        chartData: {
          labels: ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN'],
          datasets: [{ label: 'Net Profit ($)', data: [50000, 70000, 10000, 30000, 40000, 50000] }]
        },
        extraOptions: chartConfigs.lineChartOptionsBlue
      },
      userChart: {
        chartData: {
          labels: ['Europe', 'Asia', 'North America', 'Others'],
          datasets: [{ data: [35, 30, 25, 10], backgroundColor: ['#3498db', '#1abc9c', '#9b59b6', '#f39c12'] }]
        },
        extraOptions: chartConfigs.pieChartOptions
      },
      marketShareChart: {
        chartData: {
          labels: ['Nautisense', 'Competitors'],
          datasets: [{ data: [80, 20], backgroundColor: ['#f1c40f', '#e74c3c'] }]
        },
        extraOptions: chartConfigs.pieChartOptions
      }
    };
  }
};
</script>

<style scoped>
.text-center {
  text-align: center;
  margin-bottom: 20px;
}

.card-title {
  font-size: 18px;
  color: #2c3e50;
}

.card-category {
  font-size: 14px;
  color: #7f8c8d;
}
</style>

