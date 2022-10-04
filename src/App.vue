<template>
  <div class="main">
    <elevator v-for="elevator in elevators"
      :key="elevator.id"
      :floors="floorsValue"
      :current-floor="elevator.currentFloor"
    />
    <div class="blockFloors">
      <floor v-for="floor in floors.reverse()"
        :key="floor.id"
        :floor="floor.id"
        :waiting="floor.waiting"
      />
    </div>
    
  </div>
  
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import floor from '@/components/floor.vue'
import IFloor from './types/IFloor';
import elevator from '@/components/elevator.vue'
import IElevator from './types/IElevator';

export default defineComponent({
  name: 'App',
  components: {floor, elevator},
  data() {
    return {
      elevatorsValue: 1,
      floorsValue: 6,
      tasks: [],
      floors: [] as IFloor[],
      elevators: [] as IElevator[]
    }
  },
  methods: {
    floorsItems() {
      for (let i = 1; i <= this.floorsValue; i++) {
        this.floors.push({id: i, waiting: false})
      }
    },
    elevatorsItems() {
      for (let i = 1; i <= this.elevatorsValue; i++) {
        this.elevators.push({id: i, waiting: true, currentFloor: 1})
      }
    }
  },
  mounted(){
    this.floorsItems()
    this.elevatorsItems()
  }
});
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.main{
  display: flex;
}
.blockFloors {
  width: 20%;
}
</style>
