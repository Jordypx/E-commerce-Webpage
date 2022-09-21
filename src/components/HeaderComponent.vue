<template>

   <header class="body-header">
     <input type="checkbox" id="basket-check" hidden/>

 
     <div class="header-left">
        <input type="checkbox" id="topBar" hidden/>
     <span class="sidebar-toggle" for="topBar" aria-label="menu">
       <img @click="sidebarBtn()" src="../assets/Icons/icon-menu.svg" alt="">
     </span>
     <h1>
     <img src="../assets/Icons/logo.svg" alt="logo">
   </h1>
 
  
  <div class="links">
     <ul class="list-links">
       <li class="main-nav-text">
         <a href="#" class="nav-text">Collections</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">Men</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">Women</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">About</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">Contact</a>
       </li>
     </ul>
   </div>

   <nav class="sidebar-container">
   <aside class="sidebar">
     <div class="sidebar-header">
       <button class="close-btn" for="topBar" aria-label="close">
         <img @click="sidebarBtnClose()" src="../assets/Icons/icon-close.svg" alt="">
       </button>
     </div>
     <!-- links -->
     <ul class="linked">
       <li class="main-nav-text">
         <a href="#" class="nav-text">Collections</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">Men</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">Women</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">About</a>
       </li>
       <li class="main-nav-text">
         <a href="#" class="nav-text">Contact</a>
       </li>
     </ul>
   </aside>
  </nav>
   
  </div>
   
   <div class="header-right">
     <button class="cartBtn" for="basket-check" aria-label="card" @click="btnClick()">
       <svg width="22" height="20" xmlns="http://www.w3.org/2000/svg"><path d="M20.925 3.641H3.863L3.61.816A.896.896 0 0 0 2.717 0H.897a.896.896 0 1 0 0 1.792h1l1.031 11.483c.073.828.52 1.726 1.291 2.336C2.83 17.385 4.099 20 6.359 20c1.875 0 3.197-1.87 2.554-3.642h4.905c-.642 1.77.677 3.642 2.555 3.642a2.72 2.72 0 0 0 2.717-2.717 2.72 2.72 0 0 0-2.717-2.717H6.365c-.681 0-1.274-.41-1.53-1.009l14.321-.842a.896.896 0 0 0 .817-.677l1.821-7.283a.897.897 0 0 0-.87-1.114ZM6.358 18.208a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm10.015 0a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm2.021-7.243-13.8.81-.57-6.341h15.753l-1.383 5.53Z" fill="#69707D" fill-rule="nonzero"/></svg>
       <span v-if="onQuantiy > 0" class="cart-count">{{onQuantiy}}</span>
     </button>
     <div class="avatar">
        <img class="avatar" :src="avatar">
     </div>
   </div>

<div class="basket">

  <h3>Cart <img @click="BtnClose()" src="../assets/Icons/icon-close.svg" alt=""></h3>

 <p v-if="basket.length < 1" class="empty-text">Your cart is empty.</p>
 <div class="container-products-checkout">
   <div class="product-check" v-for="(product, index) in basket" :key="index">
     <img class="checkout-image" src="../assets/images/image-product-1-thumbnail.jpg">
    <div class="seperate">
     <p class="checkout-title">{{product.title}}</p>
   <div class="seperate-two">
     <p class="checkout-price">${{product.price}} x {{product.quantity}}</p>
     <span class="checkout-total">${{Number(product.price) * Number(product.quantity)}}</span>
   </div>
    </div>
     <div class="trash-icon" @click="deleteProduct(index)">
     <img src="../assets/Icons/icon-delete.svg" alt="">
     </div>
   </div>
 </div>

  <stripe-checkout 
  ref="checkoutRef"
  mode="payment"
  :pk="publishableKey"
  :line-items = "lineItems"
  :success-url = "successURL"
  :cancel-url="cancelURL"
  @loading="v => loading = v"

/>

 <button class="checkoutBtn" v-if="basket.length > 0" @click="CheckOutBtn()">Checkout</button>
</div>


</header>
</template>








<script>
import { StripeCheckout } from '@vue-stripe/vue-stripe' 
// import LogoIcon from '../assets/icons/logo.svg';
// import MenuIcon from '../assets/icons/icon-menu.svg';
// import CartIcon from '../assets/icons/icon-cart.svg';
import AvatarIcon from '../assets/images/image-avatar.png';
// import DeleteIcon from '../assets/icons/icon-delete.svg';
// import CloseIcon from '../assets/icons/icon-close.svg';
import emitter from 'tiny-emitter/instance';







export default {
   name: 'Header',
   components:{
    StripeCheckout

   },

   data(){
    this.publishableKey = 'pk_test_51Ld0W4DtscEe2fhBO9jeaYOn4auNHromThfNkFopWxypn235sR1Y7P3JeONsBKUfoMskQ70y7pZEpJsH3w4hueoz00AejKckAb'

     return {
       avatar: AvatarIcon, 
       basket:JSON.parse(localStorage.getItem("basket")) || [],
       onQuantiy:localStorage.getItem("basket") && JSON.parse(localStorage.getItem("basket")).length > 0 ? JSON.parse(localStorage.getItem("basket")).map((el)=>{
         return el.quantity
       }).reduce((a,b)=> {return a + b}) : 0,
      
       loading:false,
       lineItems:[
        {
          price: 'price_1LkORDDtscEe2fhBUijm3WCW',
          quantity:1,

        },
       ],
       successURL: 'http://localhost:8080/Success',
       cancelURL: 'http://localhost:8080/error',
     };
   
   },


   // created() {
   //   axios({
   //     method: 'get',
   //     url: 'http://localhost:9000/',
   //   })

   //    .then((response) => {
   //     this.fecth_status = "success"
   //     console.log(response.data)
   //    })
   //    .catch((error) =>{
   //     this.fecth_status = "failed"
   //    })
   // },

   mounted(){
     emitter.on('basket',()=>{
       const products = JSON.parse(localStorage.getItem("basket")) || []
       this.basket = products || []
       this.onQuantiy = products && products.length > 0 ? products.map((el)=>{
         return el.quantity
       }).reduce((a,b)=> {return a + b}) : 0
     })
   },

   methods:{
     deleteProduct(index){
       const products = JSON.parse(localStorage.getItem("basket"))
       const newList = products.filter((product) => {return product.id !== index})
       localStorage.setItem("basket", JSON.stringify(newList))
       this.basket = JSON.parse(localStorage.getItem("basket")) || []
       emitter.emit('basket',true);
     },


     btnClick() {
        document.querySelector(".basket").style.display = 'block';
     },

     BtnClose() {
        document.querySelector(".basket").style.display = 'none';
     },

     sidebarBtn() {
        document.querySelector(".sidebar-container").style.display = 'block';
     },

     sidebarBtnClose() {
        document.querySelector(".sidebar-container").style.display = 'none';
     },

     CheckOutBtn() {
      this.$refs.checkoutRef.redirectToCheckout();
    },

   }
}
</script>
