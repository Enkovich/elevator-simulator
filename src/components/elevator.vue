<template>
    <div class="blockElevator">
        <div class="floorElevator">
            <div v-for="floor in floorsInvert" :key="floor" class="blockFloorElevator">
                <div class="elevatorCabin" v-if="floor === currentFloorElevator" v-bind:class="{blockTimeout: timeoutElevator}">
                    <div class="blockElevatorDisplay">
                        <div class="blockElevatorFloorTask">{{floorTask}}</div>
                        <div class="blockElevatorDirection">{{movement}}</div> 
                    </div>
                    
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";
    export default defineComponent({
        props: {
            floors: {
                type: Number,
                required: true
            },
            floorTask: {
                type: Number,
                required: true
            },
            elevatorNumber: {
                type: Number,
                required: true
            }
        },
        computed: {
            floorsInvert(){
                let tempArray = []
                for(let i = 1; i <= this.floors; i++) {
                    tempArray.push(i)
                }
                return tempArray.reverse()
            }
        },
        data(){
            return {
                currentFloorElevator: 1,
                status: 'waiting',
                timeoutElevator: false,
                movement: ' '
            }
        },
        watch: {
            floorTask(newValue) {
                let numTimeout = 0;
                (newValue > this.currentFloorElevator) ? this.movement = '⬆':this.movement = '⬇';
                let interval = setInterval(()=>{
                    if(this.floorTask === this.currentFloorElevator) {this.status = 'timeout'; this.timeoutElevator = true}
                    if(this.status === 'waiting') (newValue > this.currentFloorElevator) ? (this.currentFloorElevator++):(this.currentFloorElevator--)
                    if(this.status === 'timeout') {
                        numTimeout++
                        this.movement = ' '
                    }
                    if(numTimeout===4) {
                        this.status = 'waiting'
                        numTimeout = 0
                        this.timeoutElevator = false
                        this.$emit('taskComplete', this.elevatorNumber, this.floorTask)
                        clearInterval(interval)
                    }
                }, 1000)
            }
        }
    })
</script>

<style lang="scss">
    .blockElevator{
        width: 120px;
        border-right: 1px solid lightgray;
        position: relative;
        
        .blockFloorElevator{
            width: 100%;
            height: 90px;
            border-bottom: 1px solid lightgray;
            position: relative;
            z-index: 1;
        }
        .elevatorCabin{
            height: 90px;
            width: 100px;
            margin: 0 auto;
            background-color: aquamarine;
            z-index: -1;
            position: relative;
            border-radius: 5px;
        }
    }
    .blockElevatorDisplay{
        display: flex;
        position: absolute;
        left: 10px;
        top: 10px;
        background-color: rgb(136, 136, 136);
        color: white;
        padding: 3px;
        border-radius: 5px;
        .blockElevatorFloorTask{
            width: 14px;
            text-align: center;
        }
        .blockElevatorDirection{
            width: 14px;
            height: 14px;
            text-align: center;
        }
    }
    .blockTimeout{
        animation: flicker 0.7s infinite;

        @keyframes flicker {
            50% {
                opacity: 0.2;
            }
            100% {
                opacity: 1;
            }
        }
    }
</style>