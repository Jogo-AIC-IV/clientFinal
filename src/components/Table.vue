<template>
    <div class="`col-12 ${owned ? 'order-1' : 'order-0'}`">
        <div class="d-flex justify-content-start align-items-center flex-row mb-1 overflow-unit">
          <TableUnit
            v-for="(enemy, enemyIndex) in enemyList" 
            :key="`table_${tableIndex}_enemy_${enemyIndex}`"
            :index="enemyIndex"
            :addClass="!owned ? 'bg-dark border-danger' : ''"
            :hiddenDetails="!owned"
            :type="enemy.type"
            :tier="enemy.attributes.tier.curr"
            :healthCurr="enemy.attributes.health.curr"
            :healthMax="enemy.attributes.health.max"
            :attack="enemy.attributes.attack.curr"
            :deffense="enemy.attributes.deffense.curr"
            :price="enemy.price"
            :gold="gold"
          />
          <TableUnit
            v-if="enemyList.length == 0"
            :hiddenDetails="!owned"
            :hidden="true"
          />
        </div>
        <div class="d-flex justify-content-start align-items-center flex-row mb-1 overflow-unit">
            <TableUnit
              v-for="(unit, unitIndex) in unitList" 
              :key="`table_${tableIndex}_unit_${unitIndex}`"
              :index="unitIndex"
              :addClass="!owned ? 'bg-dark border-danger' : ''"
              :hiddenDetails="!owned"
              :owned="owned"
              :type="unit.type"
              :tier="unit.attributes.tier.curr"
              :healthCurr="unit.attributes.health.curr"
              :healthMax="unit.attributes.health.max"
              :attack="unit.attributes.attack.curr"
              :deffense="unit.attributes.deffense.curr"
              :price="unit.price"
              :gold="gold"
              v-on:handleUpgradeUnit="handleUpgradeUnit"
            />
            <TableUnit
              v-if="unitList.length == 0"
              :hiddenDetails="!owned"
              :hidden="true"
              :owned="owned"
            />
        </div>
        <div class="row mb-3" v-if="owned">
          <div class="col-12">
            <div class="card">
                <div class="card-body shadow-sm">
                    <div class="d-flex justify-content-between">
                      <h2 class="text-start align-items-center" >
                        {{ username || 'Sem nome'}}
                        <small class="ms-2 fs-6"><font-awesome-icon class="me-2" size="1x" icon="circle-chevron-up" />{{rank}}</small>  
                      </h2>
                      <h3 class="mb-0 text-dark text-start" >{{life}}<small class="text-muted"> / 500 HP</small></h3>
                    </div>
                    <div class="progress mb-3" style="height: 20px">
                        <div class="progress-bar bg-danger progress-bar-striped progress-bar-animated" :style="`width:${lifeWidth}%;`"></div>
                    </div>
                    <div class=" d-flex justify-content-between align-items-center">
                      <h3 class="mb-0 text-muted text-start flex-1">{{gold}} <font-awesome-icon class="ms-2" size="1x" icon="coins" /></h3>
                      <button class="btn btn-success" v-on:click="handleUnitAddClick">Comprar Unidade {{unitPrice}} <font-awesome-icon class="ms-2" size="1x" icon="coins" /></button>
                    </div>
                </div>
            </div>
          </div>
        </div>
        <div class="row mb-3" v-else>
          <div class="col-12">
            <div class="card border-danger">
                <div class="card-body shadow-sm bg-dark text-white ">
                  <div class="d-flex justify-content-between">
                    <h2 class="text-start align-items-center" >
                      {{ username || 'Sem nome'}}
                      <small class="ms-2 fs-6 text-white"><font-awesome-icon class="me-2" size="1x" icon="circle-chevron-up" />{{rank}}</small>  
                    </h2>
                    <h3 class="mb-0 text-dark text-start text-white" >{{life}}<small class="text-muted"> / 500 HP</small></h3>
                  </div>
                  <div class="progress" style="height: 20px">
                      <div class="progress-bar bg-danger progress-bar-striped progress-bar-animated" :style="`width:${lifeWidth}%;`"></div>
                  </div>
                </div>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
import TableUnit from './TableUnit.vue'

export default {
  name: 'Table',
  components: {
    TableUnit
  },
  data: () => ({
  }),
  props: {
    owned:      Boolean,
    tableIndex: Number,
    enemyList:  Array,
    unitList:   Array,
    gold:       Number,
    life:       Number,
    username:   String,
    unitPrice:  Number,
    rank:       Number,
    hideUnitImages: { type: Boolean, default: false }
  },
  computed: {
    lifeWidth() {
      return Math.round(100*this.life/500);
    }
  },
  methods: {
    handleUnitAddClick() {
        this.$emit('handleUnitAddClick');
    },
    handleUpgradeUnit(index) {
        this.$emit('handleUpgradeUnit', index)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.overflow-unit {
  overflow-x: auto;
  overflow-y: visible;
}
</style>
