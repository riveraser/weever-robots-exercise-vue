<template>
<div class="cell">
    <span class="info">({{house.x + "," + house.y}})</span>
    <b-img src="img/square.png" fluid-grow></b-img>
</div>
</template>

<script>
export default {
    name: 'House',
    props:[
        'house'
    ]
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