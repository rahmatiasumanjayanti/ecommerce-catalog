<template>
  <div class="container">
    <div class="container-bg" :style="{ backgroundColor: containerBgColor }">
          
      <!-- Card -->
      <div class="card"> 
        <div>
          <!-- left --> 
          <div class="left" v-if="filteredProducts.length > 0" >
            <img :src="currentProduct.image" alt="Product Picture">
          </div>

          <!-- right -->
          <div class="right" v-if="filteredProducts.length > 0">
            <p class="title" :style="{ color: textColor }"> {{ currentProduct.title }}</p> 

            <div class="rating">
              <p class="category"> {{currentProduct.category}}</p>

              <div class="container-rating">
                  <p class="rating-img">{{currentProduct.rating.rate}}/5</p>
                  <div class="rate" >
                      <p class="bullet" :style="{ backgroundColor : buttonColor, borderColor: borderColor}"></p>
                      <p class="bullet" :style="{ backgroundColor : buttonColor, borderColor: borderColor}"></p>
                      <p class="bullet" :style="{ backgroundColor : buttonColor, borderColor: borderColor}"></p>
                      <p class="bullet" :style="{ borderColor: borderColor}"></p>
                      <p class="bullet" :style="{  borderColor: borderColor}"></p>
                  </div>
              </div>
                
            </div>

            <div>
              <p class="description">
                  {{currentProduct.description}}
              </p>

              <!-- <br><br> -->

              <p class="price" :style="{ color: textColor }">${{ currentProduct.price }}</p>
            </div>

            <div class="cover-btn" :style="{ color: textColor, color: backgroundColor }">
              <!-- button buy now -->
                <button 
                class="buy-now" 
                type="button" 
                :style="{ backgroundColor : buttonColor, borderColor: borderColor}"
                >Buy Now</button>

                <!-- button next product -->
                <button 
                class="next-product" 
                type="button" 
                @click="getNextProduct" 
                v-if="filteredProducts.length > 1" 
                :style="{ color: textColor, borderColor: borderColor }"
                >Next Product
              </button>
            </div>

          </div>
        
          <!-- Page Not Found -->
          <div v-else>
                      
            <!-- Card Sad Face -->
            <div class="card-nf">
                <div class="card-bg-nf">
                    <div class="content-nf">
                        <p>This product is unavailable to show</p>

                        <button 
                          class="next-product-nf" 
                          type="button" 
                          @click="getNextProduct">
                          <strong>Next Product</strong>
                        </button>
                    </div>
                </div>
            </div>
                          
          </div>

        </div>
      </div>
 
    </div>
  </div>
</template>

<script>
export default {
  data() {
      return {
          containerBgColor: '#FDE2FF', // warna latar belakang default
          textColor: '#720060',
          buttonColor: '#720060',
          borderColor: '#720060',
          products: [],
          categoriesToShow: ["men's clothing", "women's clothing"],
          currentProductIndex: 0
      };
  },
  mounted() {
      this.fetchData();
  },
  methods: {
      fetchData() {
          fetch('https://fakestoreapi.com/products')
              .then(response => response.json())
              .then(data => {
                  this.products = data.filter(product => this.categoriesToShow.includes(product.category));
                  // set warna latar belakang sesuai kategori produk pertama
                  this.setContainerBgColor(this.products[0].category);
              })
              .catch(error => {
                  console.error('Error fetching data:', error);
              });
      },
      getNextProduct() {
          if (this.currentProductIndex < this.filteredProducts.length - 1) {
              this.currentProductIndex++;
          } else {
              this.currentProductIndex = 0; // kembali ke produk pertama jika mencapai produk terakhir
          }
          // set warna latar belakang sesuai kategori produk saat ini
          this.setContainerBgColor(this.filteredProducts[this.currentProductIndex].category);
      },
      setContainerBgColor(category) {
          // menentukan warna latar belakang berdasarkan kategori
          this.containerBgColor = category === "women's clothing" ? '#FDE2FF' : '#D6E6FF';
          this.textColor = category === "women's clothing" ? '#720060' : '#002772';
          this.buttonColor = category === "women's clothing" ? '#720060' : '#002772';
          this.borderColor = category === "women's clothing" ? '#720060' : '#002772';
          
      }
  },
  computed: {
      filteredProducts() {
          return this.products;
      },
      currentProduct() {
          return this.filteredProducts[this.currentProductIndex];
      }
  }
}
</script>

<style scoped>
.container {
   background-color: white;
   width: 100%;
   height: 600px;
   position: absolute;
   top: 0;
}

.container-bg {
   background-color: #FDE2FF;
   background-image: url('@/assets/bg-pattern.png');
   width: 100%;
   height: 70%;
}

.card {
   position: absolute;
   top: 50%;
   left: 50%;
   transform: translate(-50%, -50%);
   padding: 30px 30px;
   width: 800px;
   height: 400px;
   background-color: white;
   border-radius: 5px;
   box-shadow: 1px 1px 10px rgb(201, 201, 201);
   overflow: auto;
}

.left {
   /* background-color: bisque; */
   width: 40%;
   height: 400px;
   float: left;
}

img {
   display: block;
   margin: auto;
   width: 85%;
}

.right {
   /* background-color: cadetblue; */
   margin-left: 20px;
   width: 55%;
   height: 400px;
   float: left;
}

p.title {
   color: #720060;
   font-size: 20px;
   font-weight: bold;
   margin: 0 0;
   word-spacing: 1px;
}

p {
   text-align: left;
   color: #1E1E1E;
}

p.price {
   color: #720060;
   font-size: 20px;
   font-weight: bold;
   margin: px 0;
}

div.rating {
   display: flex;
   color: #3F3F3F;
   font-size: 13px;
   justify-content: space-between;
   border-bottom: 1px solid rgba(0, 0, 0, 0.2);
}

p.description {
   padding-bottom: 50px;
   border-bottom: 1px solid rgba(0, 0, 0, 0.2);
   color: #1E1E1E;
}

p.rating-img {
   display: flex;
   justify-content: flex-end;
   padding-right: 5px;
}

div.rating p {
  color: #3F3F3F;
}

div.container-rating{
   display: flex;
}

div.rate {
   display: flex;
}

p.bullet {
   background-color: #720060;
   width: 12px;
   height: 12px;
   margin-right: 1px;
   border-radius: 50%;
}

p.bullet:nth-last-child(-n+2) {
   background-color: #fff;
   border: 1px solid #720060;
   width: 10px;
   height: 10px;
   border-radius: 50%;
}

.cover-btn {
  padding-bottom: 50px;
}

.buy-now {
   color: #fff;
   padding: 5px 70px;
   background-color: #720060;
   border: 1px solid #720060;
   border-radius: 3px;
   cursor: pointer;
   margin-right: 9px;
}

.next-product {
   background-color: #fff;
   color:#720060;
   padding: 5px 70px;
   border: 2px solid #720060;
   border-radius: 3px;
   cursor: pointer;
}

/* Display Not Found */

.card-bg-nf {
    background-image: url('@/assets/sad-face.jpg');
    background-repeat: no-repeat;
    background-position: center;
    background-size: 90%;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.content-nf p {
    text-align: center;
}

.next-product-nf {
    color: #3F3F3F;
    padding: 5px 150px;
    border: 2px solid #000000;
    border-radius: 3px;
    cursor: pointer;
}
</style>