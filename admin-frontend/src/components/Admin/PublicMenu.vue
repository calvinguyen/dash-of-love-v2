<script setup>
import { ref } from 'vue';
import productMenuAPI from '../../services/productMenuAPI';

/* GET ACTIVE CAKE TYPES & FLAVORS */
const cakeTypes = ref([]);
const getCakeTypes = async () => {
  try {
    const response = await productMenuAPI.getActiveCakeTypes();
    cakeTypes.value = response.data;
  } catch(err) {
    console.log(err);
  }
}
getCakeTypes();

const cakeFlavors = ref([]);
const getCakeFlavors = async () => {
  try {
    const response = await productMenuAPI.getActiveMenuFlavors();
    cakeFlavors.value = response.data;
  } catch(err) {
    console.log(err)
  }
}
getCakeFlavors();


const getFlavorsOfferedByType = (typeID) => {
  // returns a filtered array of strings (flavors)
  const typeFlavors = cakeFlavors.value.filter(flavor => (
    flavor.typeID == typeID
  )).map(( item => item.flavor ));

  return typeFlavors.join(', ');
};

</script>


<template>
<section id="menu" class="menu">
<div class="container">
  
  <div class="section-title">
    <h2>Cake <span>Menu</span></h2>
  </div>

  <div class="row menu-container">
    <div 
      class="col-lg-6 menu-item"
      v-for="cake in cakeTypes"
      :key="cake.typeID"
    >
      <div class="menu-content">
        <a>{{ cake.type }}</a><span>Starting Price: ${{ cake.price }}+</span>
      </div>
      <div class="menu-ingredients">
        Flavors: {{ getFlavorsOfferedByType(cake.typeID) }}
      </div>
    </div>
  </div>

</div>
</section>
</template>


<style scoped>
@import "../../assets/css/home.css";

.menu .menu-content {
  margin-top: 30px;
  overflow: hidden;
  display: flex;
  justify-content: space-between;
  position: relative;
}

.menu .menu-content::after {
  content: "......................................................................""....................................................................""....................................................................";
  position: absolute;
  left: 20px;
  right: 0;
  top: -4px;
  z-index: 1;
  color: #dad8d4;
  font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
}

.menu .menu-content a {
  padding-right: 10px;
  background: #fff;
  position: relative;
  z-index: 3;
  font-weight: 700;
  color: #fa0909;
}

.menu .menu-content span {
  background: #fff;
  position: relative;
  z-index: 3;
  padding: 0 10px;
  font-weight: 600;
}

.menu .menu-ingredients {
  font-style: italic;
  font-size: 14px;
  font-family: "Comic Neue", sans-serif;
  color: #948c81;
}

</style>