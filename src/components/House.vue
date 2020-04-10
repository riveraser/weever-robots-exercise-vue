<template>
<div class="cell" :style="`background-color: rgba(0,0,0, ${opacity()} );`">
    <!-- <span class="info">{{house.presents}}</span> -->
    <b-img src="img/square.png" fluid-grow></b-img>
</div>
</template>

<script>
export default {
    name: 'House',
    props:[
        'house'
    ],
    methods:{
        opacity(){
            return  (this.house.presents  * 0.1);
        }
    }
}
</script>
<style>
.info{
    position: absolute;
    top:8px;
    font-size: 8px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;

}

</style>