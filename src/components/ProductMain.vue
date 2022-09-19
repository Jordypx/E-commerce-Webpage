<template>
   <main>
  <input type="checkbox" id="lightbox" hidden>
  <div class="product-showcase">
  
  <div class="main-img">
    <label for="lightbox" aria-label="lightbox" @click="clickBtn()">
  <img :src="products_images[selected_image]" alt="image-product"/>
  </label>
  <div class="mainPrevBtn main-img-btn" @click="passImage(false)"><img src="../assets/Icons/icon-previous.svg"></div>
  <div class="mainNextBtn  main-img-btn" @click="passImage(true)"><img src="../assets/Icons/icon-next.svg" alt=""></div>
  </div>
  
  <div class="imgBtns" v-if="window_size > 1023">
    <div v-for="(thumbnail, index) in products_images_thumbs" :key="index" @click="changeImage(index)" v-bind:class="[index === selected_image ? 'image-selected' : '', 'image']">
    <img :src="thumbnail" :alt="'image-thumbnail-' + index"  />
    </div>
  </div>
  </div>
  
  <div class="lightBox-container" v-if="window_size > 1023">
  <div class="lightBox">
  
   <div class="close-lightBox" @click="labelBtn()">
    <label for="lightbox" aria-label="close"><svg width="22" height="15" xmlns="http://www.w3.org/2000/svg"><path d="m11.596.782 2.122 2.122L9.12 7.499l4.597 4.597-2.122 2.122L7 9.62l-4.595 4.597-2.122-2.122L4.878 7.5.282 2.904 2.404.782l4.595 4.596L11.596.782Z" fill="#fff" fill-rule="evenodd"/></svg></label>
   </div>
  
  <div class="gallery">
    <img :src="products_images[selected_image]"/>
  </div>
  <div class="prevBtn" @click="passImage(false)"><img src="../assets/Icons/icon-previous.svg" alt=""></div>
  <div class="nextBtn" @click="passImage(true)"><img src="../assets/Icons/icon-next.svg" alt=""></div>
  <div class="gallery-control" v-if="window_size > 1023">
    <div v-for="(thumbnail, index) in products_images_thumbs" :key="index" @click="changeImage(index)" v-bind:class="[index === selected_image ? 'image-selected' : '', 'image']">
    <img :src="thumbnail" :alt="'image-thumbnail-' + index"  />
    </div>
  </div>
  </div>
  </div>
  </main>
  </template>
  
  
  
  
  <script>
  import product_image_0 from '../assets/images/image-product-1.jpg';
  import product_image_1 from '../assets/images/image-product-2.jpg';
  import product_image_2 from '../assets/images/image-product-3.jpg';
  import product_image_3 from '../assets/images/image-product-4.jpg';
  import product_image_0_thumb from '../assets/images/image-product-1-thumbnail.jpg';
  import product_image_1_thumb from '../assets/images/image-product-2-thumbnail.jpg';
  import product_image_2_thumb from '../assets/images/image-product-3-thumbnail.jpg';
  import product_image_3_thumb from '../assets/images/image-product-4-thumbnail.jpg';
  
  export default {
     name: 'ProductMain',
  
     mounted(){
      this.$nextTick(function() {
              window.addEventListener("resize", this.changeWindow)
          })
    },
    unmounted() {
          window.removeEventListener('resize', this.changeWindow);
      },
    data(){
      return{
        products_images:[product_image_0,product_image_1,product_image_2,product_image_3],
        products_images_thumbs:[product_image_0_thumb,product_image_1_thumb,product_image_2_thumb,product_image_3_thumb],
        selected_image:0,
        window_size:window.innerWidth
      }
    },
    methods:{
      changeImage:function (index){
        this.selected_image = index
      },
      changeWindow: function(){
        this.window_size = window.innerWidth
      },
      passImage: function(index) {
        if(!index) return this.selected_image = (this.selected_image - 1 + this.products_images.length) % this.products_images.length
        this.selected_image = (this.selected_image + 1) % this.products_images.length
      },
  
      labelBtn() {
        document.querySelector(".lightBox-container").style.display = 'none';
      },
  
      clickBtn() {
        document.querySelector(".lightBox-container").style.display = 'block';
      }
    }
  
  }
  </script>
  
  
  