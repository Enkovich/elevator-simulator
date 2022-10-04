<template>
        <div class="blockFloor">
            <div class="blockFloorNumber">{{ floor }}</div>
            <div id="callElevator" 
                @click="callElevator"
                v-bind:class="{callElevator: isCall}" 
            ></div>    
        </div>  
</template>

<script lang="ts">
import { defineComponent } from 'vue';
    export default defineComponent({
        props: {
            floor: {
                type: Number,
                required: true
            },
            waiting: {
                type: Boolean,
                required: true
            }
        },
        data() {
            return {
                isCall: false,
            }
        },
        methods: {
            callElevator() {
                this.isCall = true
            }
        }
    })
</script>

<style lang="scss">
    #callElevator {
        width: 20px;
        height: 20px;
        border: 2px solid rgb(26, 139, 15);
        border-radius: 10px;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;

        &::before{
            content: '';
            display: block;
            width: 10px;
            height: 10px;
            background-color: rgb(26, 139, 15);
            border-radius: 5px;
        }
    }
    #callElevator.callElevator::before {
        background-color: rgb(218, 231, 33); 
    }
    .blockFloor{
        height: 90px;
        border-bottom: 1px solid lightgray;
        display: flex;
        align-items: center;
        padding-left: 10px;
        
        .blockFloorNumber {
            margin-right: 20px;
        }
    }
</style>