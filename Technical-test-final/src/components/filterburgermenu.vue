<template>
    <!--<span style="font-size:30px;cursor:pointer" onclick="openNav()">&#9776; open</span>-->
    <div id="myNav" class="overlay mobile-only">
        <a href="javascript:void(0)" class="closebtn" @click="closeNav()">&times;</a>
        <div class="overlay-content">
          <filterwhenmobile/>
          <div class="mobilebuttons ">
            <button class="clearbtn" @click="clearAll">Clear All</button>
            <button class="viewitembtn"  > View Items</button>
        </div>
        </div>
      </div>
    <div class="filterbtn">
        <button id="toggleButton" class="inactive btn filterationbtn" @click="openNav()">
            <i class="fa-solid fa-filter filteringicon"></i>
             Hide Filters
        </button>
    </div>
    
    
</template>

<script>
import filterwhenmobile from './filterwhenmobile.vue';
export default{
    components:{
        filterwhenmobile
    },
    data(){
        return{
            items:[],
            


        }
        
    },
    methods:{
        openNav(){
            document.getElementById("myNav").style.width = "100%";

        },
        closeNav(){
            document.getElementById("myNav").style.width = "0%";
        },
        clearAll() {
          document.getElementById("myNav").style.width = "0%";
      
    },
    
},
    
computed:{
    itemCount() {
      return this.items.length;
    },
}
}

</script>

<style>
.overlay {
    height: 100%;
    width: 0;
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    background-color:white;
    overflow-x: hidden;
    transition: 0.5s;
  }
  
  .overlay-content {
    position: relative;
    top: 25%;
    width: 100%;
    text-align: center;
    margin-top: 30px;
  }
  
  .overlay a {
    padding: 8px;
    text-decoration: none;
    font-size: 36px;
    color: #818181;
    display: block;
    transition: 0.3s;
  }
  
  .overlay a:hover, .overlay a:focus {
    color: #f1f1f1;
  }
  
  .overlay .closebtn {
    position: absolute;
    top: 20px;
    right: 45px;
    font-size: 60px;
  }

  .mobilebuttons{
    border-radius: 5px;
    display: grid;
    grid-template-columns: 50% 50%;
    padding-top:150px;
    column-gap: 20px;
    
    
  }

  .clearbtn{
    border-radius: 5px;
    border-color: #4eb0df;
    border-style: solid;
    height:50px;
    color:#4eb0df;
  }

  .viewitembtn{
    background-color:#4eb0df!important;
    border-radius: 5px;
    border-color: #4eb0df;
    border-style: solid;
    height:50px;
    color: white;

  }

  .mobile-only {
    display: none;
    /* Initially hide the component */
}

  
  @media (max-width: 844px) {
    .overlay a {font-size: 20px}
    .overlay .closebtn {
    font-size: 40px;
    top: 15px;
    right: 35px;
    }

    .filterationbtn{
        width:200px;
    }
    .mobile-only {
      display: block;
  }

  }
</style>