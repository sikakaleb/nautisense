<template>
  <card>
    <!-- En-tête de la card -->
    <template #header>
      <h4 class="card-title">Surveillance Environnementale par Zones Océaniques</h4>
      <p class="card-category">Données globales collectées</p>
    </template>

    <div class="row">
      <!-- Tableau des indicateurs par zone -->
      <div class="col-md-6 ml-auto mr-auto">
        <div class="table-responsive">
          <table class="table table-hover">
            <thead class="text-primary">
              <tr>
                <th>Zone Océanique</th>
                <th class="text-center">Température (°C)</th>
                <th class="text-center">Pollution (µg/m³)</th>
                <th class="text-center">Qualité Écosystème</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(zone, index) in oceanicZones" :key="index">
                <td>{{ zone.name }}</td>
                <td class="text-center">{{ zone.temperature }}</td>
                <td class="text-center">{{ zone.pollution }}</td>
                <td class="text-center">
                  <!-- Badge (ou icône) selon la qualité -->
                  <span
                    :class="[
                      'badge',
                      zone.ecosystemQuality === 'Très Bonne'
                        ? 'badge-success'
                        : zone.ecosystemQuality === 'Bonne'
                        ? 'badge-info'
                        : zone.ecosystemQuality === 'Moyenne'
                        ? 'badge-warning'
                        : 'badge-danger'
                    ]"
                  >
                    {{ zone.ecosystemQuality }}
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- Carte avec marqueurs de surveillance -->
      <div class="col-md-6">
        <!-- Prop :markers pour fournir une liste de points à afficher 
             (implémentation hypothétique dans votre composant AsyncWorldMap) -->
        <world-map style="height: 300px" :markers="markers"></world-map>
      </div>
    </div>
  </card>
</template>

<script>
import Loading from 'src/components/LoadingPanel.vue';

// Chargement asynchrone du composant WorldMap
const WorldMap = () => ({
  component: import('src/components/WorldMap/AsyncWorldMap.vue'),
  loading: Loading,
  delay: 200
});

export default {
  name: 'country-map-card',
  components: {
    WorldMap
  },
  data() {
    return {
      // Exemple de données de zones océaniques et d'indicateurs environnementaux
      oceanicZones: [
        {
          name: 'Atlantique Nord',
          temperature: 17.5,
          pollution: 45,
          ecosystemQuality: 'Bonne'
        },
        {
          name: 'Mer Méditerranée',
          temperature: 22.3,
          pollution: 75,
          ecosystemQuality: 'Moyenne'
        },
        {
          name: 'Océan Indien',
          temperature: 24.1,
          pollution: 30,
          ecosystemQuality: 'Très Bonne'
        },
        {
          name: 'Mer du Japon',
          temperature: 19.2,
          pollution: 65,
          ecosystemQuality: 'Moyenne'
        },
        {
          name: 'Pacifique Sud',
          temperature: 25.6,
          pollution: 40,
          ecosystemQuality: 'Bonne'
        }
      ],
      /* Exemple de marqueurs pour la carte :
         Chaque marqueur peut avoir lat/lng + un niveau de sévérité
         qui pourrait conditionner la taille ou la couleur du point
         (implémentation à gérer dans le composant AsyncWorldMap). */
      markers: [
        { lat: 40.5, lng: -30, severity: 'low' },   // Atlantique Nord
        { lat: 36,   lng: 18,  severity: 'high' },  // Méditerranée
        { lat: -20,  lng: 80,  severity: 'medium' },// Océan Indien
        { lat: 35,   lng: 140, severity: 'medium' },// Mer du Japon
        { lat: -15,  lng: -150,severity: 'low' }    // Pacifique Sud
      ]
    };
  }
};
</script>

<style scoped>
.table-responsive {
  max-height: 400px;
}
.badge {
  font-size: 0.9rem;
  padding: 5px 10px;
  border-radius: 12px;
}
</style>
