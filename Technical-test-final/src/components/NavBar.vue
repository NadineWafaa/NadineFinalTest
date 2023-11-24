<template>
  <!---1st navbar-->

  <nav class="navbar navbar-expand-lg ">
    <div class="container">
      <a class="navbar-brand logo" href="#">LOGO</a>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item">
            <a class="nav-link" href="#">
              <i class="fa-regular fa-user icon"></i> Account
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">
              <i class="fa-regular fa-heart icon"></i> Wishlist
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">
              <i class="fa-solid fa-cart-shopping icon"></i> Cart
            </a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-------2nd navbar-------->
  <nav class="navbar navbar-expand-lg">
    <div class="container">
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <a class="nav-link thing" href="#">Grocery</a>
          </li>
          <li class="nav-item">
            <a class="nav-link thing" href="#">Electronics</a>
          </li>
          <li class="nav-item">
            <a class="nav-link thing" href="#">Clothing</a>
          </li>
          <li class="nav-item">
            <a class="nav-link thing" href="#">Furniture</a>
          </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Toys & Video games</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Baby</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Beauty</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Personal Care</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Pets</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Sports</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Stationary</a>
        </li>
        <li class="nav-item">
          <a class="nav-link thing" href="#">Brands</a>
        </li>
        </ul>

        
      </div>
    </div>
  </nav>



</template>

<script>
export default {
  data() {
    return {
    
    };
  },

};
</script>

<style scoped>
.logo{
  font-weight: bold;
  font-family: 'Roboto';
}

.nav-item{
  color: black;
}

.container{
  display: flex;
  flex-direction: row;
}

.thing{
  font-weight: bold;
  font-family: 'Roboto';
  color:black;

}
</style>
