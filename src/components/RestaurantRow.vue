<template>
    <div class="restaurant--row">
        <h2 class="title">Nos restaurants</h2>
        <div class="wrapper--card">
            <RestaurantCard v-for="(card, index) in rowRestaurants" :key="index" :infoRestaurant="card" />
        </div>
    </div>
</template>

<script>
import RestaurantCard from './RestaurantCard.vue';
export default {
    name: 'RestaurantRow',
    components: {
        RestaurantCard
    },
    props: {
        rowRestaurants: Array
    }
}
</script>

<style lang="scss">
    .restaurant--row {
        .wrapper--card {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
    }
</style>