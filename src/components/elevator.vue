<template>
    <div class="blockElevator">
        <div class="floorElevator">
            <div v-for="floor in floorsInvert" :key="floor" class="blockFloorElevator">
                <div class="elevatorCabin" v-if="floor === currentFloor">{{floor}}</div>
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
            currentFloor: {
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
        }
    }
</style>