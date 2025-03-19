<template>
  <!-- Même structure que le User Table, adaptée aux capteurs -->
  <base-table :data="table" thead-classes="text-primary">
    <!-- Définition des colonnes (en-têtes du tableau) -->
    <template #columns>
      <th>#</th>
      <th>Sensor</th>
      <th>Type</th>
      <th>Status</th>
      <th class="text-right">Battery</th>
      <th class="text-right">Actions</th>
    </template>

    <!-- Corps du tableau - réutilise le slot-scope `row, index` -->
    <template #default="{ row, index }">
      <!-- Colonne # (icône/photo du capteur) -->
      <td class="text-center">
        <div class="photo">
          <img :src="row.img" alt="Sensor icon" />
        </div>
      </td>

      <!-- Nom du capteur -->
      <td>{{ row.sensorName }}</td>

      <!-- Type du capteur (Température, pH, Oxygène, etc.) -->
      <td>{{ row.type }}</td>

      <!-- Statut du capteur (Online, Offline, En Maintenance...) -->
      <td>{{ row.status }}</td>

      <!-- Pourcentage de batterie, illustré avec une barre de progression -->
      <td class="text-center">
        <base-progress :value="row.battery" />
      </td>

      <!-- Boutons d’action : par exemple Refresh / Delete -->
      <td class="text-right">
        <!-- Bouton de refresh -->
        <el-tooltip
          content="Refresh"
          effect="light"
          :open-delay="300"
          placement="top"
        >
          <base-button
            :type="index > 2 ? 'success' : 'neutral'"
            icon
            size="sm"
            class="btn-link"
          >
            <i class="tim-icons icon-refresh-01"></i>
          </base-button>
        </el-tooltip>

        <!-- Bouton de suppression -->
        <el-tooltip
          content="Delete"
          effect="light"
          :open-delay="300"
          placement="top"
        >
          <base-button
            :type="index > 2 ? 'danger' : 'neutral'"
            icon
            size="sm"
            class="btn-link"
          >
            <i class="tim-icons icon-simple-remove"></i>
          </base-button>
        </el-tooltip>
      </td>
    </template>
  </base-table>
</template>

<script>
import { BaseTable, BaseProgress } from '@/components';

export default {
  name: 'SensorTable',
  components: {
    BaseTable,
    BaseProgress
  },
  data() {
    return {
      /* Exemple de données statiques pour illustrer
         Vous pouvez évidemment récupérer ces infos depuis une API ou un store Vuex */
      table: [
        {
          id: 1,
          img: 'img/sensor1.jpg',
          sensorName: 'Temp Sensor #1',
          type: 'Température',
          status: 'Online',
          battery: 85
        },
        {
          id: 2,
          img: 'img/sensor2.jpg',
          sensorName: 'O2 Sensor #3',
          type: 'Oxygène',
          status: 'Online',
          battery: 62
        },
        {
          id: 3,
          img: 'img/sensor3.jpg',
          sensorName: 'pH Sensor #5',
          type: 'pH',
          status: 'Maintenance',
          battery: 47
        },
        {
          id: 4,
          img: 'img/sensor4.jpg',
          sensorName: 'Salinity Sensor #2',
          type: 'Salinité',
          status: 'Offline',
          battery: 0
        },
        {
          id: 5,
          img: 'img/sensor5.jpg',
          sensorName: 'CO2 Sensor #7',
          type: 'CO2',
          status: 'Online',
          battery: 73
        },
        {
          id: 6,
          img: 'img/sensor6.jpg',
          sensorName: 'Turbidity Sensor #4',
          type: 'Turbidité',
          status: 'Online',
          battery: 58
        }
      ]
    };
  }
};
</script>

<style scoped>
/* Si besoin, ajustez le style pour adapter l'affichage */
.photo img {
  width: 40px;
  height: 40px;
  object-fit: cover;
  border-radius: 50%;
}
</style>
