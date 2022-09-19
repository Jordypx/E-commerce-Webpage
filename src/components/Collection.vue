<template>

   <div class="ProductMainclass">
   
         <div class="MainEvn">
           <ProductMain />
         </div>
   
         <div class="product-info" role="contentinfo">
               <span class="company-name">Sneaker Company</span>
               <h1 class="main-text">{{product.title}}</h1>
               <p class="description">{{product.description}}</p>
               <div class="product-priceBox">
                   <p class="price">${{product.price}} <span class="discount">{{product.offer}}</span></p>
                   
                   <p class="old-price">${{product.price_original}}</p>
               </div>
               <div class="add-cart-container">
                   <div class="counter">
                       <button aria-label="minus" @click="quantity_product(false)" class="countMinus">
                           <img src="../assets/Icons/icon-minus.svg" alt="">
                       </button>
                       <p class="count">{{product.quantity_sell}}</p>
                       <button @click="quantity_product(true)" aria-label="sum" class="countPlus">
                          <img src="../assets/Icons/icon-plus.svg" alt="">
                       </button>
                   </div>
                   <button class="addCartBtn" @click="addProduct()" aria-label="cart"><div><svg width="22" height="20" xmlns="http://www.w3.org/2000/svg"><path d="M20.925 3.641H3.863L3.61.816A.896.896 0 0 0 2.717 0H.897a.896.896 0 1 0 0 1.792h1l1.031 11.483c.073.828.52 1.726 1.291 2.336C2.83 17.385 4.099 20 6.359 20c1.875 0 3.197-1.87 2.554-3.642h4.905c-.642 1.77.677 3.642 2.555 3.642a2.72 2.72 0 0 0 2.717-2.717 2.72 2.72 0 0 0-2.717-2.717H6.365c-.681 0-1.274-.41-1.53-1.009l14.321-.842a.896.896 0 0 0 .817-.677l1.821-7.283a.897.897 0 0 0-.87-1.114ZM6.358 18.208a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm10.015 0a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm2.021-7.243-13.8.81-.57-6.341h15.753l-1.383 5.53Z" fill="#fff" fill-rule="nonzero"/></svg></div>  Add to cart</button>
               </div>
           </div>
   
   </div>
      
   </template>
   
   
   
   <script>
   import ProductMain from '../components/ProductMain.vue';
   import emitter from 'tiny-emitter/instance';
   
   export default {
      name: 'Collection',
      components: {
       ProductMain,
      },
       
       data(){
           return{
               product:{
                   id:0,
                   title:"Fall Limited Edition Sneakers",
                   description:`These low-profile sneakers are your perfect casual wear companion. Featuring a durable rubber outer sole, they’ll withstand everything the weather can offer.`,
                   price:"125.00",
                   offer:"50%",
                   price_original:"250.00",
                   quantity_sell:0,
                   image:'../assets/images/image-product-1.jpg'
               }
           }
       },
       methods:{
           quantity_product(i){
               if (i) {
                   this.product.quantity_sell += 1
               }else{
                   this.product.quantity_sell < 1 ? this.product.quantity_sell = 0 : this.product.quantity_sell -= 1
               }
           },
   
           addProduct(){
               if(this.product.quantity_sell <= 0){
                   alert("You need to specify the quantity to buy")
                   return
               }
               const basket_local = localStorage.getItem("basket")
               let product = {id:this.product.id, title:this.product.title, price:this.product.price, quantity:this.product.quantity_sell, image:this.product.image}
               if(!basket_local){
               localStorage.setItem("basket",JSON.stringify([product]))
               }else{
                   let array_products = JSON.parse(localStorage.getItem('basket'));
                   let product_exist = array_products.filter((el)=>{ return el.id == product.id})
                       if(product_exist.length == 0 || !product_exist){
                           array_products.push(product)
                           localStorage.setItem("basket", JSON.stringify(array_products))
                       }else{
                           let product_override = array_products.filter((el)=>{el.id !== product.id})
                           product_override.push(product)
                           localStorage.setItem("basket", JSON.stringify(product_override))
                   }
               }
               this.product.quantity_sell = 0
               emitter.emit('basket',true);
           }
       }
     }
   
   </script>