<template>
  <!-- On réutilise <base-table> et la mise en page "title / description" -->
  <base-table :data="tableData" thead-classes="text-primary">
    <template slot-scope="{ row }">
      <!-- Checkbox pour marquer l’alerte comme “traitée” ou “résolue” -->
      <td>
        <base-checkbox v-model="row.done"></base-checkbox>
      </td>

      <!-- Informations principales de l’alerte -->
      <td>
        <p class="title">{{ row.title }}</p>
        <p class="text-muted">{{ row.description }}</p>
      </td>

      <!-- Icône d’action (Edition ou détail) -->
      <td class="td-actions text-right">
        <el-tooltip
          content="Voir / Éditer l'alerte"
          effect="light"
          :open-delay="300"
          placement="top"
        >
          <base-button type="link">
            <i class="tim-icons icon-pencil"></i>
          </base-button>
        </el-tooltip>
      </td>
    </template>
  </base-table>
</template>

<script>
import { BaseTable, BaseCheckbox, BaseButton } from '@/components';

export default {
  name: 'TaskList', // Vous pouvez renommer en "AlertList" si souhaité
  components: {
    BaseTable,
    BaseCheckbox,
    BaseButton
  },
  data() {
    return {
      /* Exemple de données d’alertes sur les capteurs
         (à adapter selon votre logique métier ou en flux temps réel) */
      tableData: [
        {
          title: 'Température anormale détectée',
          description: 'Capteur #1 - Lecture à 29°C (seuil max 25°C).',
          done: false
        },
        {
          title: 'Niveau de CO₂ trop élevé',
          description: 'Capteur CO₂ (#3) - Taux détecté : 410 ppm.',
          done: false
        },
        {
          title: 'Maintenance requise sur capteur de pH',
          description:
            'Dernière calibration remontant à plus de 72h, pH capteur #5.',
          done: true
        },
        {
          title: 'Rejet anormal d’hydrocarbures',
          description:
            'Échantillon #4 - Valeur au-dessus du seuil: 2.5 mg/L (limite : 1 mg/L).',
          done: false
        },
        {
          title: 'Capteur de salinité hors-ligne',
          description: 'Capteur #7 - Aucun signal depuis 20 minutes.',
          done: false
        },
        {
          title: 'Sonar - valeur hors plage attendue',
          description:
            'Sonar #2 - Écho inhabituel détecté à 12:34 (coordonnées X:Y).',
          done: false
        }
      ]
    };
  }
};
</script>


<style scoped>
.title {
  font-weight: bold;
}
.text-muted {
  font-size: 0.9rem;
}
</style>
