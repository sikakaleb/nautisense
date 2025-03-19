<template>
  <div class="fixed-plugin" v-click-outside="closeDropDown">
    <div class="dropdown show-dropdown" :class="{ show: isOpen }">
      <a data-toggle="dropdown" class="settings-icon">
        <i class="fa fa-cog fa-2x" @click="toggleDropDown"> </i>
      </a>
      <ul class="dropdown-menu" :class="{ show: isOpen }">
        <li class="header-title">Sidebar Background</li>
        <li class="adjustments-line">
          <a class="switch-trigger background-color">
            <div class="badge-colors text-center">
              <span
                v-for="item in sidebarColors"
                :key="item.color"
                class="badge filter"
                :class="[`badge-${item.color}`, { active: item.active }]"
                :data-color="item.color"
                @click="changeSidebarBackground(item);"
              ></span>
            </div>
            <div class="clearfix"></div>
          </a>
        </li>

        <li class="header-title">Sidebar Mini</li>
        <li class="adjustments-line">
          <div class="togglebutton switch-sidebar-mini">
            <span class="label-switch">OFF</span>
            <base-switch
              v-model="sidebarMini"
              @input="minimizeSidebar"
            ></base-switch>
            <span class="label-switch label-right">ON</span>
          </div>

          <!-- Switch pour passer du mode clair au mode sombre -->
          <div class="togglebutton switch-change-color mt-3">
            <span class="label-switch">LIGHT MODE</span>
            <base-switch v-model="darkMode" @input="toggleMode"></base-switch>
            <span class="label-switch label-right">DARK MODE</span>
          </div>
        </li>

        <li class="button-container mt-4">
          <!-- Vous pouvez ajouter d'autres boutons ou éléments ici -->
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { BaseSwitch } from 'src/components';

// EXPLICATION : 
// - On part du principe que "darkMode = false" au chargement => fond blanc
// - Si l’utilisateur active le switch => darkMode = true => supprime la classe 'white-content'

export default {
  name: 'sidebar-share',
  components: {
    BaseSwitch
  },
  props: {
    backgroundColor: String
  },
  data() {
    return {
      sidebarMini: true,
      // Définir darkMode à false pour que le fond soit blanc par défaut :
      darkMode: false,
      isOpen: false,
      sidebarColors: [
        { color: 'primary', active: false, value: 'primary' },
        { color: 'vue', active: true, value: 'vue' },
        { color: 'info', active: false, value: 'blue' },
        { color: 'success', active: false, value: 'green' },
        { color: 'warning', active: false, value: 'orange' },
        { color: 'danger', active: false, value: 'red' }
      ]
    };
  },
  mounted() {
    // Comme darkMode = false par défaut,
    // on force l’ajout de la classe 'white-content' au body s’il n’y est pas déjà
    document.body.classList.add('white-content');
  },
  methods: {
    toggleDropDown() {
      this.isOpen = !this.isOpen;
    },
    closeDropDown() {
      this.isOpen = false;
    },
    toggleList(list, itemToActivate) {
      list.forEach(listItem => {
        listItem.active = false;
      });
      itemToActivate.active = true;
    },
    changeSidebarBackground(item) {
      this.$emit('update:backgroundColor', item.value);
      this.toggleList(this.sidebarColors, item);
    },
    // Méthode pour gérer le switch light/dark
    toggleMode(isDark) {
      const docClasses = document.body.classList;
      if (isDark) {
        // Si darkMode = true => on supprime la classe white-content (fond noir)
        docClasses.remove('white-content');
      } else {
        // Si darkMode = false => on ajoute la classe white-content (fond blanc)
        docClasses.add('white-content');
      }
    },
    minimizeSidebar() {
      this.$sidebar.toggleMinimize();
    }
  }
};
</script>
<style scoped lang="scss">
  @import '~@/assets/sass/dashboard/custom/variables';

  .settings-icon {
    cursor: pointer;
  }

  .badge-vue {
    background-color: $vue;
  }
</style>
