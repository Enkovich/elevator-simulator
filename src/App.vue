<template>
  <div class="main">
    <elevator v-for="elevator in elevators"
      :key="elevator.id"
      :elevatorNumber="elevator.id"
      :floors="floorsValue"
      :floorTask="elevator.currentFloor"
      @taskComplete="taskComplete"
    />
    <div class="blockFloors">
      <floor v-for="floor in floors"
        :key="floor.id"
        :floor="floor.id"
        :waiting="floor.waiting"
        @callElevator="callElevator"
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
      //количество лифтов
      elevatorsValue: 3,
      //количество этажей
      floorsValue: 8,
      tasks: [] as Number[],
      floors: [] as IFloor[],
      elevators: [] as IElevator[],
      elevatorsCurrentFloors: [] as Number[]
    }
  },
  methods: {
    floorsItems() {
      let tempArray:IFloor[] = []
      for (let i = 1; i <= this.floorsValue; i++) {
        tempArray.push({id: i, waiting: false})
        if(!localStorage.getItem('elevatorsCurrentFloors')) this.elevatorsCurrentFloors.push(1)
      }
      tempArray.reverse()
      this.floors = tempArray
    },
    elevatorsItems() {
      if(localStorage.getItem('elevators')) {
        let tempString = localStorage.getItem('elevators')
        if(tempString) this.elevators = JSON.parse(tempString)
        if(this.elevators.length !== 0) return false
      }
      for (let i = 1; i <= this.elevatorsValue; i++) {
        this.elevators.push({id: i, waiting: true, currentFloor: 1})
      }
      this.saveData()
    },
    callElevator(floorNum:number){
      let i = 0
      let searchElevator = true

      if(!this.elevatorsCurrentFloors.includes(floorNum)) {
        while(i<=this.elevators.length-1) {
          if(this.elevators[i].waiting === true) {
            this.elevatorsCurrentFloors = this.elevatorsCurrentFloors.filter((elem)=> {return elem !== this.elevators[i].currentFloor})
            this.elevators[i] = {id: this.elevators[i].id, waiting: false, currentFloor: floorNum}
            this.elevatorsCurrentFloors.push(floorNum)
            i=i+this.elevators.length
            searchElevator = false
          }
          i++
        }
        this.floors[this.floors.length - floorNum].waiting = true
        
        if(!searchElevator) return false
        this.tasks.push(floorNum)
        this.saveData()
      }
    },
    taskComplete(elevatorNum:number, floorTask:number)  {
      this.floors[this.floors.length - floorTask].waiting = false
      if(this.tasks.length!==0) {
        
        this.elevators[elevatorNum-1] = {id: this.elevators[elevatorNum-1].id, waiting: false, currentFloor: Number(this.tasks[0])}
        this.elevatorsCurrentFloors.push(this.tasks[0])
        this.elevatorsCurrentFloors = this.elevatorsCurrentFloors.filter((elem)=>{return elem !== floorTask})
        this.tasks = this.tasks.filter((elem) => {return elem !== this.tasks[0]})
      } else {
        this.elevators[elevatorNum-1] = {id: this.elevators[elevatorNum - 1].id, waiting: true, currentFloor: floorTask}
      }
      console.log(this.tasks, this.elevatorsCurrentFloors) 
      this.saveData()
    },
    saveData(){
      localStorage.elevators = JSON.stringify(this.elevators)
      localStorage.elevatorsCurrentFloors = JSON.stringify(this.elevatorsCurrentFloors)
    }
  },
  computed: {},
  mounted(){
    this.floorsItems()
    this.elevatorsItems()
    if(localStorage.getItem('elevatorsCurrentFloors')) {
      let tempString = localStorage.getItem('elevatorsCurrentFloors')
      if(tempString) this.elevatorsCurrentFloors = JSON.parse(tempString)
    }
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