<template>
    <div class="iv-increment-button iv-drop-shadow-medium" style="height:100%" >
        <button id="subtract" class="iv-button" style="height:100%" @click="minusClick" :disabled="minusDisabled">-</button>
        <span class="text">{{current}}</span>
        <button id="add" class="iv-button" style="height:100%" @click="plusClick"  :disabled="plusDisabled">+</button>
    </div>

</template>

<script>
import {eventBus} from "@/buses/eventBus";
export default {
    name: "iv-increment-button",
    props:{
      plusDisabled: {
        type:Boolean,
        required: false,
        default: false
      },

      minusDisabled: {
        type:Boolean,
        required: false,
        default: false
      },

      initialValue:{
          type:Number,
          required:false,
          default:1
      },

      increment:{
          type:Number,
          required:false,
          default:1
      },

      minimum:{
          type:Number,
          required:false,
      },
      maximum:{
          type:Number,
          required:false,
      },


    },
    data(){
      return {
        current: this.initialValue
      }
    },
    methods:{
        mouseOn(e){
            this.$emit("mouseover",e)
        },
        mouseOff(e){
            this.$emit("mouseleave",e)
        },
        minusClick(){
            if (this.current==this.maximum){
                this.plusDisabled=false;
            }
            
            this.current = this.current-this.increment;
            this.$emit("change", this.current);

            if (this.current==this.minimum){
                this.minusDisabled=true
            }
        },
        
        plusClick(){
            if (this.current==this.minimum){
                this.minusDisabled=false
            }
            
            this.current = this.current+this.increment;
            this.$emit("change", this.current);

            if (this.current==this.maximum){
                this.plusDisabled=true;
            }
        }
    },

    mounted(){
      eventBus.$on("reset-data", data => {
        console.log(data);
        this.current = this.initialValue;
        this.minusDisabled=false;
        this.plusDisabled=false;
      });
    }
  
}
</script>

<style lang="scss">
@import "src/globals";
$green:#19CF86;
$greenHover:#1BDE90;
$greenClick:#139C65;
$disabledColor:#D3D3D3;


.iv-increment-button{
    border: none;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 1.2rem;
    height:1.45rem;
    padding: 0.25em 0.5em;
    font-weight:500;
}

.text{
    padding-right:0.5em;
    padding-left:0.5em;
}


.iv-button{
    border: none;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 1.2rem;
    height:1.45rem;
    padding: 0.25em 0.5em;
    background-color: $green;
    color:$white;
    font-weight:500;
    transition: all 0.2s;
    &:focus{
        outline:0;
    }

    &:hover{
        background-color:$greenHover;
    }
    &:active{
        background:$greenClick;
        color:#F0FFF0;
    }


    &:disabled{
        background-color:$disabledColor;
        color:#a3a3a3;
    }
}


</style>