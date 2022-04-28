<template>
    <div class="home">
        <div class="header">
            <img src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" alt="Logo Uber Eats">

            <div class="wrapper--input">
                <input v-model="userSearch" type="text" placeholder="De quoi avez-vous envie ?">
                <div class="search">
                    <router-link v-for="(restaurant, i) in filteredRestaurants" :key="i" :to="{name: 'Restaurant', params : { name: restaurant.name }}">
                        <div class="container--restaurant--search">
                            <div class="wrapper--img">
                                <img :src="restaurant.image" :alt="i">
                            </div>
                            <h2>{{ restaurant.name }}</h2>
                        </div>
                    </router-link>
                </div>
            </div>
        </div>
        <div class="banner"></div>

        <RestaurantRow v-for="(data, i) in restaurants" :key="i" :rowRestaurants="data" />
    </div>
</template>

<script>
import RestaurantModel from '../services/RestaurantModel';
import RestaurantRow from '../components/RestaurantRow.vue';
import { onMounted, ref, watch } from 'vue';

export default {
    name: 'Home',
    components: {
        RestaurantRow
    },
    setup() {
        class Restaurant {
            constructor(name, note, image, drive_time) {
                this.name = name;
                this.note = note;
                this.image = image;
                this.drive_time = drive_time;
            }
        }

        // ref permet d'auto-update tout ce qui est lié à elle lorsqu'elle change
        let restaurants = ref([]);
        let noOrderRestaurants = [];

        const fetchRestaurants = () => {
            let rowRestaurants = [];

            for (const restaurantData of RestaurantModel) {
                const restaurant = new Restaurant(restaurantData.name, restaurantData.note, restaurantData.image, restaurantData.drive_time);
                rowRestaurants.push(restaurant);
                noOrderRestaurants.push(restaurant);

                 if (rowRestaurants.length === 3) {
                    restaurants.value.push(rowRestaurants);

                    rowRestaurants = [];
                }
            }

            if (rowRestaurants.length > 0) {
                restaurants.value.push(rowRestaurants);
            }
        }

        let userSearch = ref('');
        let filteredRestaurants = ref([]);

        // Dès que userSearch est modifié, le callback est appelé
        watch(userSearch, newValue => {
            if (newValue.length === 0) {
                filteredRestaurants.value = [];
            } else {
                const regex = RegExp(newValue.toLowerCase());

                filteredRestaurants.value = noOrderRestaurants.filter(restaurant => regex.test(restaurant.name.toLowerCase()));
            }

            
        });
        
        onMounted(fetchRestaurants);

        return { 
            restaurants,
            userSearch,
            filteredRestaurants
        }
    }
}
</script>

<style lang="scss">
.home {
    .header {
        height: 120px;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;

        img {
            width: 200px;
        }

        .wrapper--input {
            position: relative;

            input {
                background-color: #f6f6f6;
                border: none;
                width: 400px;
                height: 60px;
                outline: none;
                padding: 0 10px;
            }

            .search {
                position: absolute;
                top: 100%;
                width: 100%;
                background-color: #f6f6f6;

                .container--restaurant--search {
                    display: flex;
                    align-items: center;
                    padding: 10px;

                    &:hover {
                        background-color: #fff;
                    }

                    .wrapper--img {
                        height: 60px;
                        width: 60px;
                        margin-right: 25px;
                        border-radius: 50%;
                        overflow: hidden;

                        img {
                            height: 100%;
                            width: auto;
                        }
                    }
                }
            }
        }
    }

    .banner {
        height: 200px;
        width: 100%;
        background-image: url("https://www.ubereats.com/restaurant/_static/7b308f7cbbf8e335ceda0447a8bd7c63.png");
        background-size: cover;
        background-position: center;
    }
}
</style>