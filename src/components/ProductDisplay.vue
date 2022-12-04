<template>
    <div v-if="loading === true">
        <div class="background">
            <div class="card">
                <div class="loader"></div>
            </div>
        </div>
    </div>
    <div v-else>
        <div v-if="data">
            <div v-if="gender === 'male'">
                <div class="background male">
                    <div class="card">
                        <div class="product-image">
                            <img :src="data.image" :alt="data.title"/>
                        </div>
                        <div class="product-overview">
                            <div class="product-detail-top">
                                <div class="product-name">
                                    <h1>{{ data.title }}</h1>
                                </div>
                                <div class="category-rating">
                                    <div class="product-category">
                                        <p>
                                            {{ data.category }}
                                        </p>
                                    </div>
                                    <div class="product-rating">
                                        <div>{{ data.rating.rate }}/5</div>
                                        <div class="rating">
                                            <div class="rating-scale" v-for="countRating in 5" :key="countRating">
                                                <div class="rating-fill male-fill" v-if="countRating <= ratingScale"></div>
                                                <div class="rating-fill male-no-fill" v-else></div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="line"><hr></div>
                                <div class="product-description">
                                    <p>
                                        {{ data.description }}
                                    </p>
                                </div>
                            </div>
                            <div class="line"><hr></div>
                            <div class="product-detail-bottom">
                                <div class="product-price">
                                    <h2>${{data.price}}</h2>
                                </div>
                                <div class="button">
                                    <button class="male-buy-button male-fill">Buy Now</button>
                                    <button class="male-next-button" @click.prevent="countProduct">Next product</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-else>         
                <div class="background female">
                    <div class="card">
                        <div class="product-image">
                            <img :src="data.image" :alt="data.title"/>
                        </div>
                        <div class="product-overview">
                            <div class="product-detail-top">
                                <div class="product-name">
                                    <h1>{{ data.title }}</h1>
                                </div>
                                <div class="category-rating">
                                    <div class="product-category">
                                        <p>
                                            {{ data.category }}
                                        </p>
                                    </div>
                                    <div class="product-rating">
                                        <div>{{ data.rating.rate }}/5</div>
                                        <div class="rating">
                                            <div class="rating-scale" v-for="countRating in 5" :key="countRating">
                                                <div class="rating-fill female-fill" v-if="countRating <= ratingScale"></div>
                                                <div class="rating-fill female-no-fill" v-else></div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="line"><hr></div>
                                <div class="product-description">
                                    <p>
                                        {{ data.description }}
                                    </p>
                                </div>
                            </div>
                            <div class="line"><hr></div>
                            <div class="product-detail-bottom">
                                <div class="product-price">
                                    <h2>${{ data.price }}</h2>
                                </div>
                                <div class="button">
                                    <button class="female-buy-button female-fill">Buy Now</button>
                                    <button class="female-next-button" @click.prevent="countProduct">Next product</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-else>
            <div class="background unavailable">
                <div class="card frown">
                    <div class="content-unavailable">
                        <p>This product is unavailable to show</p>
                        <button class="button-unavailable" @click.prevent="countProduct">Next product</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ProductDisplay",
    data() {
        return {
            data: [],
            loading: true,
            qtyProduct: 1,
            gender: "",
            countRating: 1,
            ratingScale: "",
            description: ""
        };
    },
    mounted() {
        this.getData();
    },
    methods: {
        countProduct() {
            if (this.qtyProduct === 20) {
                this.qtyProduct = 1;
                console.log(this.qtyProduct);
                this.getData();
            } else {
                this.qtyProduct = this.qtyProduct + 1;
                console.log(this.qtyProduct);
                this.getData();
            }
        },
        getData() {
            this.loading = true;
            setTimeout(() => {
                fetch("https://fakestoreapi.com/products/" + this.qtyProduct)
                .then(response => response.json())
                .then(data => {
                    this.loading = false;
                    this.data = data;
                    if (this.data.category === `men's clothing` || this.data.category == `women's clothing`) {
                        if (this.data.category === `men's clothing`) {
                            this.gender = "male";
                            console.log(this.gender);
                            this.data = data;
                            this.ratingScale = Math.round(this.data.rating.rate);
                        } else if (this.data.category == `women's clothing`) {
                            this.gender = "female";
                            console.log(this.gender);
                            this.data = data;
                            this.ratingScale = Math.round(this.data.rating.rate);
                            this.description = this.data.description;
                        }
                    } else {
                    this.data = false;
                    }
                })
                .catch(err => console.log(err.message));
            }, 1000);
        }
    },
};
</script>

<style scoped>@import "../assets/style/page.css";</style>