<template>
  <v-navigation-drawer
    v-model="drawer"
    absolute
    temporary
    :style="{ 'z-index': 9999 }"
    v-show="machineDataManager.machines.length"
  >
    <v-list
      nav
      dense
    >
      <v-list-item :class="{
        primary: active === machine.id,
      }" link v-for="machine in machineDataManager.machines" :key="machine.id" @click="load(machine)">
        <v-list-item-icon>
          <v-chip class="job text-center" small label :class="{
            primary: active === machine.id,
          }">
            {{ machine.preview.split('/')[1][0] }}
          </v-chip>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{machine.name || '(未命名)'}}</v-list-item-title>
          <v-list-item-subtitle>{{`${machine.preview.split('/')[0]}/${machine.preview.split('/')[1]}`}}</v-list-item-subtitle>
          <v-list-item-subtitle>{{ new Date(machine.timestamp).toLocaleDateString() }} {{new Date(machine.timestamp).toLocaleTimeString()}}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
  name: "SavedMachineList",
  props: {
    machineDataManager: {
      type: Object,
    },
  },
  data() {
    return {
      active: -1,
      drawer: false,
    }
  },
  methods: {
    load(machine) {
      this.drawer = false;
      if (this.active === machine.id) {
        this.$emit('close');
        return;
      }
      this.active = machine.id;
      this.$emit('load', machine);
    },
  },
};
</script>

<style>
.job {
  min-width: 36px;
  text-align: center;
}
</style>