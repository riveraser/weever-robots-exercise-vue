<template>
    <div class="robot" 
        :style="`background-color:${robot.color}; left: ${left}; top: ${top};`"
        v-b-tooltip.hover :title="robot.name + ' | Presents: ' + robot.presents "
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
                let newPos = 50 + (1.96 * this.robot.x);
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
        },
        moveUp(){
            this.robot.y -= 1;
        },
        moveRight(){
            this.robot.x += 1;
        },
        moveDown(){
            this.robot.y += 1;
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