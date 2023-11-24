<template>
    <div class="filterdisplay">
        <div class="filterrec">
            <div class="fil">{{categoryTitle}}</div>
            <i class="fa-solid fa-x xicon"></i>
        </div>
    </div>

</template>

<script>
export default{
    props:{
        categoryTitle: String
    }
}
</script>

<style>
.filterrec {
    display: flex;
    flex-direction: row;
    width:120px;
    justify-content: center;
    height:30px;
    align-items: center;
    border-style: solid;
    border-width:1px;
    border-color: black;
    border-radius:5px;
    margin-left:20px;

}

.xicon {
    
    padding-left :5px;
    
}



</style>