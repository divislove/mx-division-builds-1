<template>
  <div id="app">
    <BasicTile classes="toolbar-container">
      <Toolbar></Toolbar>
    </BasicTile>
    <div class="grid-container" v-if="loaded">
      <router-view></router-view>
      <WeaponStats></WeaponStats>
      <div class="spec-and-stats">
        <BasicTile classes="specialization">
          <SpecializationSlot></SpecializationSlot>
        </BasicTile>
        <BasicTile classes="general-stats-col no-anim">
          <GeneralStats></GeneralStats>
        </BasicTile>
      </div>
    </div>
    <v-dialog />
  </div>
</template>

<script>
import { allDataPromies } from "./utils/dataImporter";
import WeaponStats from "./components/WeaponStats/WeaponStats";
import GeneralStats from "./components/GeneralStats";
import BasicTile from "./components/BasicTile";
import SpecializationSlot from "./components/SpecializationSlot";
import Toolbar from "./components/Toolbar";
export default {
  name: "App",
  components: {
    WeaponStats,
    GeneralStats,
    BasicTile,
    SpecializationSlot,
    Toolbar
  },
  data() {
    return {
      loaded: false
    };
  },
  created() {
    Promise.all(allDataPromies).then(() => {
      this.loaded = true;
      console.log("allDataPromies Complete");
    });
  }
};
</script>

<style lang="scss">
@import "../node_modules/ag-grid-community/dist/styles/ag-grid.css";
@import "../node_modules/ag-grid-community/dist/styles/ag-theme-alpine-dark.css";
@import "./style/main.scss";
@import "./style/_include-media.scss";

html,
body,
#app {
  margin: 0;
  background: #252525;
  background: #232830;
  // font-family: "Roboto", sans-serif;
  font-family: "Titillium Web", sans-serif;
  font-weight: 400;
  color: white;
  margin-bottom: 8px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(5, [col] 1fr);
  grid-template-rows: repeat(2, [row] auto);
  gap: 8px;
  margin-top: 8px;
  margin-left: 8px;
  margin-right: 8px;
  @include media("<=uibreak") {
    grid-template-columns: repeat(1, [col] 1fr);
    .inventory-gui {
      grid-column: col 1;
      grid-row: row 1;
    }
    .spec-and-stats {
      grid-column: col 1;
      grid-row: row 2;
    }
    .general-stats-col.tile {
      position: relative;
    }
    .weapon-stats-container {
      grid-column: col 1;
      grid-row: row 3;
    }
  }
}

.inventory-gui {
  grid-column: col / span 3;
  grid-row: row;
}

.specialization {
  grid-column: col 4 / span 2;
  grid-row: row;
  margin-bottom: 8px;
}

.weapon-stats-container {
  grid-column: col / span 3;
  grid-row: row 2;
}

.spec-and-stats {
  grid-column: col 4 / span 2;
  grid-row: row;
}

.tile.general-stats-col {
  position: sticky;
  top: 0;
  height: fit-content;
}

.tile.toolbar-container {
  min-height: 0px !important;
  padding: 8px;
  margin: 8px;
}

.no-element-selected {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.25);
  cursor: pointer;
  p {
    position: absolute;
    bottom: 16px;
    left: 0px;
    right: 0px;
    color: orange;
    text-align: center;
  }
}

.talent-description {
  padding: 10px;
  border-bottom: 1px solid white;
}

.tile:not(.specialization) {
  min-height: 230px;
}
</style>
