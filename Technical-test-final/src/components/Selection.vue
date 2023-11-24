<template>
    <!--<div class="way" v-for="item in path">
        <div class="flow"> {{ item.choice1}} / {{item.choice2}} / {{item.choice3}}</div>
      </div>-->
      <div class="way" >
        <div class="flow" >
            <div class="choice" >Home</div>
            <div class="slash">/</div>
            <div class="choice" >Beauty</div>
            <div class="slash">/</div>
            <div class="choicelast" >Cosmetics</div>
        </div>
        <div class="select"  >Cosmetics</div>
      </div>
</template>

<script>
export default {
  data() {
    return {
      
    }
  },
  
}

</script>

<style>
.way{
    padding-left: 30px;
    padding-top:30px;
}

.flow{
    display: flex;
    flex-direction: row;
    font-family: 'Roboto';
    font-size:15px;
}

.choice{
    padding-right:10px;
    padding-left: 10px;
}

.choicelast {
  color:rgb(160, 158, 158);
  padding-right:10px;
    padding-left: 10px;
}

 .slash{
  color:rgb(160, 158, 158);
 }
.select{
    font-family: 'Roboto';
    font-size:25px;
    font-weight: bold;
    padding-top:20px;


}



</style>