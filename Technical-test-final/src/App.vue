<script>

import { RouterLink, RouterView } from 'vue-router'



export default {


    components: {
        RouterLink, RouterView
    }
};

</script>

<template>
    <div id="routing">
        <router-link to="/"></router-link>

    </div>
    <router-View />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inspiration&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

* {
    box-sizing: border-box;
}

/*:root {
    --nav: #12232e;
    --lightblue: #007cc7;
    --darkblue: #203647;
    --background: #f4f4f4;
    --active: #abb5be;
    --light: white;
}*/

body {
    padding: 0;
    margin: 0;
    font-family: "Roboto", sans-serif;
    background-color: var(--background);
}
</style>
