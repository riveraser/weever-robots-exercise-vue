<template>
    <div class="robot" 
        :style="`background-color:${robot.color}; left: ${left}; top: ${top};`"
        v-b-tooltip.hover :title="robot.name"
    >
    </div>
</template>

<script>
export default {
    name: 'Robot',
    props:[
        "robot"
    ],
    data: function() {
        return {
        }
    },
    computed: {
        left(){
            if ( this.robot.x ==0)
                return "calc(50% - 4px)";
            else {
                let newPos = 50 - (1.96 * this.robot.x);
                return `calc(${newPos}% - 4px)`;
            }
        },
        top(){
            if ( this.robot.y ==0)
                return "calc(50% - 4px)";
            else {
                let newPos = 50 + (1.96 * this.robot.y);
                return `calc(${newPos}% - 4px)`;
            }
        }
    },
    methods:{
        moveLeft(){
            this.robot.x -= 1;
            console.log( `Moving ${this.robot.name} to the left`);
        },
        moveUp(){
            this.robot.y -= 1;
            console.log( `Moving ${this.robot.name} up`);
        },
        moveRight(){
            this.robot.x += 1;
            console.log( `Moving ${this.robot.name} to the right`);
        },
        moveDown(){
            this.robot.y += 1;
            console.log( `Moving ${this.robot.name} to down`);
        },
    }
}
</script>

<style>
    .robot{
        position: absolute;
        width: 9px;
        height: 9px;
        z-index: 10;
        cursor: pointer;
        transition: all .2s ease-out;
    }
</style>