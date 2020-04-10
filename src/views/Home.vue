<template>
  <b-container fluid class="bv-example-row">
    <b-row>
      <b-col cols="9" class="test" >
        <div class="grid">
          <Robots v-for="robot in robots" :key="robot.name" :robot="robot"
            :ref="'robot' + robot.name"
          ></Robots>
          <House v-for="house in houses" :key="house.id" :house="house" ></House>
        </div>
      </b-col>
      <b-col cols="3" class="pt-5" >
        <b-form @submit.prevent="onSubmit" @reset.prevent="onReset">
          <div v-if="!isRunning">
            <b-form-group
                  id="input-group-1"
                  label="Robot name:"
                  label-for="input-1"
                  description="Chars only, separated by comas to add more"
                >
                  <b-form-input
                    id="robotNames"
                    v-model="robotNames"
                    type="text"
                    required
                    placeholder="Robot Names"
                    @keypress="charsOnly($event)"
                  ></b-form-input>
                </b-form-group>

                <b-form-group
                  id="input-group-2"
                  label="Movements:"
                  label-for="movements"
                  description="We only accept: ^><v "
                >
                  <b-form-input
                    id="movements"
                    v-model="movements"
                    type="text"
                    required
                    placeholder="Movements"
                    @keypress="charAllowed($event)"
                  ></b-form-input>
                </b-form-group>
              </div>
              <b-button v-if="!isRunning" type="submit" variant="primary" :disabled="!canStart" class="mr-1" >Submit</b-button>
              <b-button type="reset" variant="danger">Reset</b-button>
            </b-form>
        <h4 class="mt-2" >Robots:</h4>
        <div v-for="robot in robots" :key="robot.name" class="text-left mb-2" >
          <b-avatar size="sm" :style="`background-color:${robot.color}`" ></b-avatar>
          {{robot.name}} - <small class="text-muted">Actual position: ({{robot.x + ", "+ robot.y}})</small>
        </div>
        <div>
          <h5>Movements:</h5>
          <div class="row px-1"  >
             <b-avatar v-for="(moves, index) in movementsArr" :key="index" size="sm" 
              :variant="getAvatarVariant(index)"
              :text="moves" class="mr-1" ></b-avatar>
          </div>
        </div>
      </b-col>
    </b-row>
</b-container>
</template>

<script>
// @ is an alias to /src
import House from '@/components/House'
import Robots from '@/components/Robots'

export default {
  name: 'Home',
  components: {
    House,
    Robots
  },
  data: function(){
    return {
      colors:[
        "red",
        "gold",
        "darkkhaki",
        "yellow",
        "green",
        "olive",
        "turquoise",
        "teal",
        "deepskyblue",
        "mediumblue",
        "navy",
        "fuchsia",
        "darkorchid",
        "hotpink",
        "hotpink"
      ],
      houses: [],
      robotNames:'Bob',
      robots: [
        {
          name: "Bob",
          color: "red",
          x: 0,
          y: 0,
          round: 0
        }
      ],
      movements: '',
      movementsArr:[],
      currentRound: 0,
      currentRobot: 0,
      isRunning: false
    }
  },
  mounted(){

    let col = 1;
    let row = 1;
    
    let maxGridQuarter= 26;

    //lets create a grid of 51 by 51
    for (let i = 0; i < (51 * 51); i++){
      
      let newHouse = {
        id: i,
        isOccupied: false,
        col: col,
        row: row,
        x: 0,
        y: 0
      }
      newHouse.x = col - maxGridQuarter;
      newHouse.y = row - maxGridQuarter;

      col++;
      if (col > 51){
        col=1
        row++;
      }
      this.houses.push(newHouse);
    }
  },
  computed:{
    //Can we start the Simulation?
    canStart(){
      if (this.robots.length > 0 && this.movements.length >0 )
        return true;
      else
        return false;
    }
  },
  methods: {
      onReset(evt){
        this.robotNames = "Bob";
        this.movements ="";
        this.movementsArr = [];
        this.currentRound = 0;
        this.currentRobot = 0;
        this.isRunning = false;
      },
       onSubmit(evt) {
          evt.preventDefault();
          this.movementsArr = this.movements.split("");
          let currentBot = this.robots[this.currentRobot];
          let refRobot = this.$refs['robot' + currentBot.name][0];

          //Do we have more moves?
          if (this.currentRound <  this.movementsArr.length){
          
            switch(this.movementsArr[this.currentRound]){
              case ">":
                refRobot.moveLeft();
                break;
              case "<":
                refRobot.moveRight();
                break;
              case "V":
              case "v":
                refRobot.moveDown();
                break;
              case "^":
                refRobot.moveUp();
                break;
            }
            this.currentRound++;
            this.currentRobot++;
          }

          //We reset the robots
          if (this.currentRobot >= this.robots.length){
            this.currentRobot = 0;
          }
          
          this.isRunning = true;

      },
      charAllowed(evt){
        evt = (evt) ? evt : window.event;
        var charCode = (evt.which) ? evt.which : evt.keyCode;
        // > : 62
        // < : 60
        // v : 118 // V: 86
        // ^ : 94
        
        switch(charCode){
          case 62:
          case 60:
          case 118:
          case 86:
          case 94:
            return true;
            break;
          default:
            evt.preventDefault();
            return false;
        }
      },
      charsOnly(evt){
        evt = (evt) ? evt : window.event;
        var charCode = (evt.which) ? evt.which : evt.keyCode;
        
        if ((charCode >= 65 && charCode <= 90) || (charCode >= 97 && charCode <= 122) || charCode == 44){
          return true;
        } else {
          evt.preventDefault();
          return false;
        }
      },
      
      getAvatarVariant(index){
        if (index == this.currentRound - 1){
          return "primary";
        } else if ( index < this.currentRound - 1 ){
          return "success";
        } else {
          return "light";
        }
      }
    },
    watch: {
      //Lets watch for the Robot names
      robotNames (newVal, oldVal){
        
        this.robots=[];

        if (newVal != ""){
          let robotNames = newVal.split(",");
          for(let i=0; i < robotNames.length; i++){
            let newRobot = {
                name: robotNames[i],
                color: this.colors[i],
                x: 0,
                y: 0,
                round: 0
            }
            this.robots.push(newRobot);
          }

        } 
      }
    }
}
</script>
<style lang="less">
  .test{
    border: 1px solid red;
  }
  .test1{
    border: 1px solid blue;
  }
  .grid{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;

    margin: 10px;
    margin-right: 0;
    position: relative;
    border: none;
    border-right: 1px solid black;
    border-bottom: 1px solid black;
    line-height: 0;
    .row{
      margin-right: 0;
      margin-left: 0;
    }
    .cell{
      position: relative;
      width: 1.96%;
      border-left: 1px solid black;
      border-top: 1px solid black;
    }
  }
.disabled{
  opacity: 0.3!important; 
}
</style>