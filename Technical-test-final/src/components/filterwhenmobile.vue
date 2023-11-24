<template>
    <div class="wholebody mobile-only">
        <div class="filterby">
            <div class="filtertitle">Filter By</div>
        </div>
        <div class="choices">
            <div class="edit show categories">
                <div class="categorydisplay">
                    <div class="category">
                        <div class="categorytitle">Category</div>
                        <button @click="toggleCategoryDetails" class="ms-12">
                            {{ showCategoryDetails ? '-' : '+' }}
                        </button>
                    </div>
                    <div class="searchbar">
                        <i class="fas fa-search"></i>
                        <input type="text" placeholder="Search by Category" v-model="searching"
                            @input="performSearchOnCategory" />
                        <ul>
                            <li v-for="result in searchResults" :key="result.id">
                                {{ result.title }}
                            </li>
                        </ul>
                    </div>
                    <div v-show="showCategoryDetails" v-for="item in items" :key="item.id">
                        <label class="container">
                            <input type="checkbox" @click="applyFilterOnCategories(item.id)"  />
                            {{ item.title }}
                        </label>
                    </div>
                </div>
            </div>
            <div class="edit showbrand">
                <div class="branddisplay">
                    <div class="brand">
                        <div class="brandtitle">Brand</div>
                        <button @click="toggleBrandDetails" class="ms-12">
                            {{ showBrandDetails ? '-' : '+' }}
                        </button>
                    </div>

                    <div class="searchbar">
                        <i class="fas fa-search"></i>
                        <input type="text" placeholder="Search by Brand" v-model="searching2"
                            @input="performSearchOnBrand" />
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
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
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
        }

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
    }
}

</script>

<style>
.wholebody {
    display: flex;
    flex-direction: column;
}

.filterby {
    display: flex;
    flex-direction: row;
}

.filtertitle {
    font-weight: bold;
    font-family: 'Roboto';
}

.mobile-only {
    display: none;
    /* Initially hide the component */
}

.choices {
    display: flex;
    flex-direction: column;
}

.filtertitle {
    font-family: 'Roboto';
    font-size: 30px;
    margin-top: -200px;
    margin-left: 30px;
}

.edit {
    margin-left: 30px;
    margin-top: -50px;
}

@media (max-width: 844px) {
    .mobile-only {
        display: block;
    }
}
</style>