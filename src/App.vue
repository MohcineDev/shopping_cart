<script setup>
import { ref } from "vue";

const isActive = ref(false);
const quantity = ref(0)
let totalPrice = ref(0);

let products = ref([
  {
    id: 1,
    name: "product 1",
    img: "./1.png",
    price: 2400,
  },
  {
    id: 2,
    name: "product 2",
    img: "./2.png",
    price: 2000,
  },
  {
    id: 3,
    name: "product 3",
    img: "./img.jpeg",
    price: 2600,
  },
  {
    id: 4,
    name: "product 4",
    img: "./img.jpeg",
    price: 2000,
  },
  {
    id: 5,
    name: "product 5",
    img: "./1.png",
    price: 1600,
  },
  {
    id: 6,
    name: "product 6",
    img: "./2.png",
    price: 1800,
  },
]);

let listCards = ref([]);

const addToCart = (key) => {
  if (listCards.value[key] == null) {
    listCards.value[key] = JSON.parse(JSON.stringify(products.value[key]));
    listCards.value[key].quantity = 1;
    quantity.value++
  }
  reloadCard();
};
///[...,1]
const reloadCard = () => {
  totalPrice.value = 0
  listCards.value.forEach((value, i)  =>  {
      totalPrice.value += parseInt(value.price)
  })
};

function changeQuantity(cardId, qty) {
  if (qty == 0) {
    delete listCards.value[cardId];
    quantity.value -=1
  } else { 
      listCards.value[cardId].quantity = qty;
      listCards.value[cardId].price = qty * products.value[cardId].price;
  }
 reloadCard();
}
</script>

<template>
  <div class="container" :class="{ active: isActive }">
    <header>
      <h1>shopping</h1>
      <div class="shopping" @click="isActive = true">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M15.75 10.5V6a3.75 3.75 0 10-7.5 0v4.5m11.356-1.993l1.263 12c.07.665-.45 1.243-1.119 1.243H4.25a1.125 1.125 0 01-1.12-1.243l1.264-12A1.125 1.125 0 015.513 7.5h12.974c.576 0 1.059.435 1.119 1.007zM8.625 10.5a.375.375 0 11-.75 0 .375.375 0 01.75 0zm7.5 0a.375.375 0 11-.75 0 .375.375 0 01.75 0z"
          />
        </svg>

        <span class="quantity">{{ quantity }}</span>
      </div>
    </header>

    <div class="list">
      <div class="item" v-for="(product, i) in products" :key="product.id">
        <img :src="product.img" />
        <div class="title">{{ product.name }}</div>
        <div class="price">{{ product.price.toLocaleString() }}</div>
        <button @click="addToCart(i)">Add To Cart</button>
      </div>
    </div>
  </div>
  <div class="card" :class="{ active: isActive }">
    <h1>Card</h1>
    <ul class="listCard" v-if="listCards.length > 0">
      <li v-for="(card, i) in listCards" :key="i">
        <template v-if="card != null">
          <div>
            <img :src="card.img" />
          </div>
          <div class="cardTitle">{{ card.name }}</div>
          <div class="cardPrice">{{ card.price.toLocaleString() }}</div>
          <div class="btns">
            <button
              class="cardButton"
              @click="changeQuantity(i, card.quantity - 1)"
            >
              -
            </button>
            <div class="count">{{ card.quantity }}</div>
            <button
              class="cardButton"
              @click="changeQuantity(i, card.quantity + 1)"
            >
              +
            </button>
          </div>
        </template>
      </li>
    </ul>
    <div class="checkout">
      <div class="total">{{ totalPrice }}</div>
      <div class="closeShopping" @click="isActive = false">Close</div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 1200px;
  margin: auto;
  transition: 0.5s;
}

h1 {
  color: hsla(160, 100%, 37%, 1);
}
header {
  display: flex;
  margin-top: 50px;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
}
.shopping {
  position: relative;
  text-align: right;
}
.shopping svg {
  width: 30px;
  height: 30px;
  cursor: pointer;
}
span {
  position: absolute;
  top: -10px;
  left: 20px;
  padding: 5px;
  font-size: 14px;
  color: #fff;
  background-color: #ea1a1a;
  border-radius: 20%;
}
.list {
  display: grid;
  grid-template-columns: repeat(3, 250px);
  gap: 20px;
  margin: 50px 0;
  justify-content: center;
}

.card {
  position: fixed;
  top: 0;
  left: 100%;
  width: 500px;
  background-color: #dadada;
  border-left: 1px solid var(--green);
  height: 100vh;
  transition: 0.5s;
}
.card.active {
  left: calc(100% - 500px);
}
.container.active {
  transform: translateX(-200px);
}
.card h1 {
  color: var(--col1);
  font-weight: 300;
  margin: 0;
  padding: 0 20px;
  height: 80px;
  display: flex;
  align-items: center;
}

ul {
  padding: 0;
}
li {
  margin: 5px;
}
.card .checkout {
  position: absolute;
  bottom: 0;
  width: 100%;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}
.card .checkout div {
  background-color: var(--col1);
  color: #fff;
  width: 100%;
  height: 70px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  cursor: pointer;
}

.card .checkout div:nth-child(2) {
  background-color: #000;
  color: #fff;
}
.list .item {
  text-align: center;
  background: #efefef;
  padding: 20px;
  box-shadow: 0 15px 15px #aaa;
  letter-spacing: 1px;
  transition: all 0.25s ease;
  border-radius: 8px;
  cursor: pointer;
  transition: box-shadow 0.3s;
}

.list .item:hover {
  box-shadow: 0 10px 10px #aaa;

}
.list .item img {
  width: 90%;
}
.list .item .title {
  font-weight: 600;
}
.price {
  margin: 10px;
}

.item button {
  background-color: #efefef;
  color: var(--green);
  font-size: 1rem;
  font-weight: 600;
  width: 100%;
  padding: 10px;
  border: 3px solid var(--green);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.25s ease;
}
 
.listCard li {
  display: grid;
  grid-template-columns: 100px repeat(3, 1fr);
  align-items: center;
  justify-items: center;
}
.listCard .btns {
  display: flex;
  justify-content: center;
  align-items: center;
}
.listCard li img {
  width: 90%;
}
.listCard .count {
  margin: 0px 10px;
  color: #1f1f25;
}
.cardTitle,
.cardPrice {
  color: #1f1f25;
}
.cardButton {
  font-size: 1.5rem;
  font-weight: 500;
  border-radius: 50%;
  background-color: transparent;
  cursor: pointer;
  border: none;
}
</style>
