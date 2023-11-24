<template>
  <nav-bar></nav-bar>
  <Selection></Selection>
  <div class="main">
    <!----start of sidebar------>
    <div class="filters">

      <!-------Side Bar Category---------->
      <div class="categorydisplay">
        <div class="category">
          <div class="categorytitle">Category</div>
          <button @click="toggleCategoryDetails" class="ms-12">
            {{ showCategoryDetails ? '-' : '+' }}
          </button>
        </div>
        <div class="searchbar">
          <i class="fas fa-search"></i>
          <input type="text" placeholder="Search by Category" v-model="searching" @input="performSearchOnCategory" />
          <ul>
            <li v-for="result in searchResults" :key="result.id">
              {{ result.title }}
            </li>
          </ul>
        </div>
        <div v-show="showCategoryDetails" v-for="item in items" :key="item.id">
          <label class="container">
            <input type="checkbox" @click="applyFilterOnCategories(item.id)" />
            {{ item.title }} ( {{ itemCount }} )
          </label>
        </div>
      </div>



      <!--End of Category-->

      <!------Side Bar Brands------->
      <div class="branddisplay">
        <div class="brand">
          <div class="brandtitle">Brand</div>
          <button @click="toggleBrandDetails" class="ms-12">
            {{ showBrandDetails ? '-' : '+' }}
          </button>
        </div>

        <div class="searchbar">
          <i class="fas fa-search"></i>
          <input type="text" placeholder="Search by Brand" v-model="searching2" @input="performSearchOnBrand" />
          <ul>
            <li v-for="result in searchResults" :key="result.id">
              {{ result.title }}
            </li>
          </ul>
        </div>
        <div v-show="showBrandDetails" v-for="item in brands" :key="item.id">
          <label class="container">
            <input type="checkbox" @click="applyFilterOnBrands(item.id)" />
            {{ item.title }}
          </label>
        </div>
      </div>





      <!-------End of Brand----->

    </div>
    <!-----End of sidebar-------->


    <!--right part of The website-->
    <div class="rigthbody">
      <!-------show filter part------->
      <div class="fil">
        <filterburgermenu />
        <filteredchoice>{{ categoryTitle }}</filteredchoice>
      </div>


      <!-----show product part------->
      <div v-if="products.length > 0">
        <div class="displays">
          <!---display Products part without filter-->
          <div class="products" v-for="product in products">
            <div class="image" :key="product.default_variant.image">
              <img :src="product.default_variant.image" class="card-img-top" alt="..." />
            </div>
            <div class="info">
              <div class="name" :key="product.title">{{ product.title }}</div>
              <div class="price" :key="product.default_variant.price">
                {{ product.default_variant.price }}
              </div>
            </div>
          </div>
          <!-----end of display products----->
        </div>
      </div>

      <!--End of show products------>

      <!--------Pagination-->
      <div class="row numbers mt-5 mb-5 text-center">
        <v-pagination active-color="black" color="black" v-model="currentPage" :length="6"></v-pagination>
      </div>
      <!--------->
    </div>
  </div>
</template>

<script>
import NavBar from "../components/NavBar.vue";
import Selection from "../components/Selection.vue";
import filterburgermenu from "../components/filterburgermenu.vue";
import { VPagination } from "vuetify/components/VPagination";
import filteredchoice from "../components/filteredchoice.vue";
import VueFilter from "vue-filter";
import axios from "axios";
import Filterburgermenu from "../components/filterburgermenu.vue";
export default {
  name: "ShopPageView",
  components: {
    NavBar,
    Selection,
    filteredchoice,
    Filterburgermenu
  },
  data() {
    return {
      showCategoryDetails: false,
      showBrandDetails: false,
      items: [
        {
          title: "",
        },
      ],
      brands: [
        {
          title: "",
        },
      ],
      products: [
        {
          imageSrc: "",
          title: "",
          price: "",
        },
      ],
      currentPage: 1,
      searching: "",
      searching2: '',
      searchResults: [],
      temp: [],
      selectedCategories: [],
      flag: false,
      TempProd: [],

    };
  },
  props: {
    categoryTitle: String,
  },
  beforeMount() {

    //get categories titles for sidebar
    axios
      .get("https://joulia.dashboard.hamburgermenu.app/api/v1/categories")

      .then((response) => {
        this.items = response.data.data;
        this.temp = response.data.data;
        // console.log(response.data.data);
      })
      .catch((err) => {
        // Handle errors
        console.log(err);
      });



    //get brands titles for sidebars
    axios
      .get("https://joulia.dashboard.hamburgermenu.app/api/v1/brands")
      .then((response) => {
        this.brands = response.data.data;
        //console.log(response.data.data)
      })
      .catch((err) => {
        // Handle errors
        console.log(err);
      });



    //get all products for the body
    axios
      .get("https://joulia.dashboard.hamburgermenu.app/api/v1/products")
      .then((response) => {
        this.products = response.data.data;
        this.TempProd = response.data.data;
        //console.log(response.data.data)
      })
      .catch((err) => {
        // Handle errors
        console.log(err);
      });
  },
  methods: {

    //when checking the checkbox in category
    applyFilterOnCategories(category_id) {
      //if it's the first time i need to remove all products
      if (this.flag == false) {
        axios
          .get(
            `https://joulia.dashboard.hamburgermenu.app/api/v1/products?filter[categories]=${category_id}`
          )

          .then((response) => {
            this.products = response.data.data;

            //console.log(category_id)
            // console.log(response.data.data)
            this.selectedCategories.push(category_id);
          })
          .catch((err) => {
            // Handle errors
            console.log(err);
          });

        this.flag = true;
      }
      //Unchecked
      else if (this.selectedCategories.includes(category_id)) {
        this.selectedCategories = this.selectedCategories.filter(
          (item) => item != category_id
        );
        //All Categories unchecked
        console.log(this.selectedCategories.length);
        if (this.selectedCategories.length == 0) {
          this.products = this.TempProd;
        }
      }
      //Multiple Categories checked
      else {
        axios
          .get(
            `https://joulia.dashboard.hamburgermenu.app/api/v1/products?filter[categories]=${category_id}`
          )

          .then((response) => {
            // this.products.push(response.data.data);
            this.products = [...this.products, ...response.data.data];
            // console.log(this.products)
            //console.log(category_id)
            //console.log(response.data.data)
            this.selectedCategories.push(category_id);
          })
          .catch((err) => {
            // Handle errors
            console.log(err);
          });
      }
    },

    //when checking the checkbox in brands
    applyFilterOnBrands(brand_id) {
      if (this.flag == false) {
        axios
          .get(
            `https://joulia.dashboard.hamburgermenu.app /api/v1/products?filter[brands]=${brand_id}`
          )
          .then((response) => {
            this.products = response.data.data;
            //console.log(response.data.data)
          })
          .catch((err) => {
            // Handle errors
            console.log(err);
          });
      } else if (this.selectedBrands.includes(brand_id)) {
        this.selectedBrands = this.selectedBrands.filter(
          (item) => item != brand_id
        );
        //All Categories unchecked
        console.log(this.selectedBrands.length);
        if (this.selectedBrands.length == 0) {
          this.products = this.TempProd;
        }
      }
      //Multiple Categories checked
      else {
        axios
          .get(
            `https://joulia.dashboard.hamburgermenu.app /api/v1/products?filter[brands]=${brand_id}`
          )

          .then((response) => {
            // this.products.push(response.data.data);
            this.products = [...this.products, ...response.data.data];
            // console.log(this.products)
            //console.log(category_id)
            //console.log(response.data.data)
            this.selectedBrands.push(category_id);
          })
          .catch((err) => {
            // Handle errors
            console.log(err);
          });
      }
    },



    //search bar in categories
    performSearchOnCategory() {
      if (this.searching == "") {
        this.items = this.temp;
      } else {
        this.items = this.temp.filter((item) =>
          item.title.toLowerCase().includes(this.searching.toLowerCase())
        );
      }
    },

    //search bar in brands
    performSearchOnBrand() {
      if (this.searching2 == "") {
        this.items = this.temp;
      } else {
        this.items = this.temp.filter((item) =>
          item.title.toLowerCase().includes(this.searching2.toLowerCase())
        );
      }
    },

    // + and - sign in categories
    toggleCategoryDetails() {
      this.showCategoryDetails = !this.showCategoryDetails;

    },

    // + and - sign in brands
    toggleBrandDetails() {
      this.showBrandDetails = !this.showBrandDetails;

    },
    fetchItems() {
      // Replace 'https://api.example.com/items' with your actual API endpoint
      axios.get(`https://joulia.dashboard.hamburgermenu.app/api/v1/products?filter[categories]=${category_id}`)
        .then(response => {
          // Assign the fetched items to the 'items' data property
          this.items = response.data;

          // Now, this.itemCount will automatically update based on the length of the items array
        })
        .catch(error => {
          console.error('Error fetching items:', error);
        });


    },

  },

  computed: {

    //for pagination
    totalPages() {
      const filteredSpaces = this.spaces.filter((space) =>
        space.name.toLowerCase().includes(this.searchTerm.toLowerCase())
      );
      return Math.ceil(filteredSpaces.length / this.spacesPerPage);
    },
    itemCount() {
      // Calculate the count based on the length of the items array
      return this.items.length;
    },
  }
}

</script>

<style>
.main {
  display: grid;
  grid-template-columns: 33.33% 66.66%;
  grid-column: 1/3;
}

.filters {
  padding-left: 20px;
  padding-top: 70px;
}

.heading {
  font-family: "Roboto";
  font-size: 20px;
  font-weight: bold;
}

.searchbar {
  margin-top: 15px;
  margin-bottom: 20px;
}

i {
  color: rgb(190, 190, 190);
}

.brand {
  margin-top: 100px;
}

.displays {
  display: grid;
  grid-template-columns: auto auto auto;
  column-gap: 35px;
  row-gap: 100px;
}

.products {
  display: flex;
  flex-direction: column;
}

.image {
  width: 200px;
  height: 150px;
}



.info {
  padding-top: 150px;
  /*text-align: center;*/
  font-family: "Roboto";
  font-size: 15px;
}

.price {
  font-weight: bold;
}

.rigthbody {
  display: flex;
  flex-direction: column;
}

.fil {
  display: flex;
  flex-direction: row;
}

.category,
.brand {
  display: flex;
  flex-direction: row;
  font-weight: bold;
  font-family: 'Roboto';
  font-size: 25px;

}



@media (max-width: 844px) {

  .filters {
    display: none;
  }

  .displays {
    display: grid;
    grid-template-columns: auto auto;
    row-gap: 20px;

  }

  .image {
    width: 170px;
    height: 75px;
  }


}
</style>
