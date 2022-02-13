<template>
  <div class="img-show" >
<!----shift left--->
      <div class="icon-left" @click="shifLeft">
            <img src="https://image.ibb.co/mRsEb7/left_arrow.png" alt="alt">
      </div>
<!----Start Product--->
      <div class="product-container" >
          <div class="boxx img-main img-main-left" :class="{ loading }" :loading="loading" v-for="(picture, index) in products"  :key="index" >
              <a :href="localePath(`/p/${productGetters.getId(picture)}/${productGetters.getSlug(picture)}`)" >
                <SfImage  class="sf-product "  :src="picture.images[0].url" alt="alt" />
                <h3 class="related-product-price">{{$n(productGetters.getFormattedPrice(productGetters.getPrice(picture).regular), 'currency')}}</h3>
              </a>
          </div>

      </div>
<!----End Product--->
          <!----shift right--->
          <div class="icon-right" @click="shifRight">
              <img src="https://image.ibb.co/dfPSw7/right_arrow.png" alt="alt">
          </div>

  </div>
</template>
<script>
import SfImage from "@storefront-ui/vue/src/components/atoms/SfImage/SfImage.vue";
import SfArrow from "@storefront-ui/vue/src/components/atoms/SfArrow/SfArrow.vue";
import { productGetters, useWishlist, wishlistGetters, useCart } from '@vue-storefront/commercetools';
import { computed } from '@vue/composition-api';
export default {
name:"ImgShow",
components:{
  SfImage,
  SfArrow,
},
  setup() {
    const { addItem: addItemToCart, isInCart } = useCart();
    const { addItem: addItemToWishlist, isInWishlist, removeItem: removeItemFromWishlist, wishlist } = useWishlist();
    const removeProductFromWishlist = (productItem) => {
      const productsInWhishlist = computed(() => wishlistGetters.getItems(wishlist.value));
      const product = productsInWhishlist.value.find(wishlistProduct => wishlistProduct.variant.sku === productItem.sku);
      removeItemFromWishlist({ product });
    };
    return {
      productGetters,
      addItemToWishlist,
      isInWishlist,
      removeProductFromWishlist,
      addItemToCart,
      isInCart
    };
  },
mounted(){
this.shifRight();
this.shifRight();
},
methods: {
  shifRight() {
    const boxes = document.querySelectorAll(".boxx");
    const NuBoxes = boxes.length;
    var IndexOfBoxes = boxes.length-1 ;
    let i= 0;
    for(i=0;i<NuBoxes;i++){
      if(boxes[i].className =="boxx img-main img-main-left" || boxes[i].className =="boxx img-main img-main-right" ){

          if(i == IndexOfBoxes){
            break;
          }
          else if(i == IndexOfBoxes-1){
                for(let j=0 ; j<NuBoxes ;j++){
                    if(j!=IndexOfBoxes || j!=IndexOfBoxes-1 || j!=0){
                      boxes[j].className = "boxx img-disappear";
                    }
                }

                boxes[IndexOfBoxes].className ="boxx img-main img-main-right";
                boxes[IndexOfBoxes-1].className = "boxx img-1-left-out-main";
                boxes[0].className = "boxx img-2";

                break;
          }
          else{
                for(let j=0 ; j<NuBoxes ;j++){
                  if(j!=i+1 || j!=i+2 || j!=i){
                    boxes[j].className = "boxx img-disappear";
                  }
                }
                boxes[i+1].className = "boxx img-main img-main-right";
                boxes[i].className ="boxx img-1-left-out-main";
                boxes[i+2].className = "boxx img-2";
                break;
          }


      }
    }
  },


  shifLeft(){
    const boxes = document.querySelectorAll(".boxx");
    const NuBoxes = boxes.length;
    var IndexOfBoxes = boxes.length-1 ;
    let i= 0;
    for(i=0;i<NuBoxes;i++){
      if(boxes[i].className =="boxx img-main img-main-left" || boxes[i].className =="boxx img-main img-main-right" ){
          if(i == 0){ //first image of list so prevent moving left
            break;
          }
          else if(i == 1){//from second image movin to first image of list
                for(let j=0 ; j<NuBoxes ;j++){
                  if(j!=0 || j!=IndexOfBoxes || j!=1){
                    boxes[j].className = "boxx img-disappear";
                  }
                }
                boxes[0].className ="boxx img-main img-main-left"; //set first image as main image
                boxes[IndexOfBoxes].className = "boxx img-1"; //bring last image in array to first
                boxes[1].className = "boxx img-2-right-out-main";//move second image forward
                break;
          }
          else{
                for(let j=0 ; j<NuBoxes ;j++){
                  if(j!=i-1 || j!=i-2 || j!=i){
                    boxes[j].className = "boxx img-disappear";
                  }
                }
                boxes[i-1].className = "boxx img-main img-main-left";
                boxes[i].className ="boxx img-2-right-out-main";
                boxes[i-2].className = "boxx img-1";
                break;
          }


      }
    }
  },

},
props: {
    title: String,
    products: Array,
    loading: Boolean,
    alt: {
      type: String,
      required: true,
    },
  },

}
</script>
<style lang="scss">

.img-show{
  height: 500px;
  width: 100%;
  margin-left: 10%;
  position: relative;
  display: flex;
  flex-direction: row;



.related-product-price{
  color: var(--_c-paige-primary);
  font-family: var(--font-family--text-containe);
  font-size: var(--font-size--lg);
  letter-spacing: 0.4px;
  @include for-mobile{
    text-align: left;
    margin-left: 14%;
    font-size: small;
}
  }
.product-container{
    width: 100%;
    position: relative;
    width: 60%;
    height: 100%;
    display:flex;
    margin-left: 7%;
   .img-1,.img-2{
    width: 150px;
    position: absolute;
    height: 220px;
    z-index: 2;
    opacity: 0.7;
    @include for-mobile{
      display: none;
    }
    img{
      width: 150px;
      height: 220px;
      border-radius: 4%;
    }

  }
  ///Image 1
  .img-1{
      left: 0;
      margin-left: 0%;

  }
    .img-1-left-out-main{
      transition: 0.5s ease-in-out;
      animation: middleLeftOutMain  0.5s 0s forwards;
      @include for-mobile{
        display: none;
      }
      img{
        height: 100%;
        width:100%;
        border-radius: 4%;
      }
      @keyframes middleLeftOutMain {
        0%{
              position: absolute;
              left:15%;
              width:225px;
              height:325px;
              opacity: 0.7;
              z-index: 2;

        }
        100%{
              position: absolute;
              width: 150px;
              height: 220px;
              left: 0%;
              z-index: 2;
              opacity: 0.7;
        }
      }
  }
  //Image 2
  .img-2{
      right: 14%;
      margin-left: 0;
  }
  .img-2-right-out-main{
        transition: 0.5s ease-in-out;
        animation: middleRightOutMain  0.5s 0s forwards;
        @include for-mobile{
          display: none;
        }
          img{
            height: 100%;
            width:100%;
            border-radius: 4%;

          }
        @keyframes middleRightOutMain {
          0%{
                position: absolute;
                right: 35%;
                width:225px;
                height:325px;
                opacity: 0.7;
                z-index: 2;

          }
          100%{
                position: absolute;
                width: 150px;
                height: 220px;
                right: 15%;
                z-index: 2;
                opacity: 0.7;

          }
    }
  }
.img-main-left{
    transition: 0.5s ease-in-out;
    position: absolute;
    @include for-desktop{
    animation: middleMain  1s 0s forwards;
    }
      @include for-mobile{
      animation: middleMainMobile  1s 0s forwards;
      display: block;
      width: 100%;
      height: 100%;

      }
    img{
      height: 100%;
      width:100%;
      border-radius: 4%;
      @include for-mobile{
      width: 50%;
      }
    }
    @keyframes middleMain {
      0%{
            width: 150px;
            height: 220px;
            left: 0;
            z-index: 3;
            opacity: 0.8;
      }
      100%{
            left: calc(var(--spacer-3xl) - var(--spacer-2xl));
            width:300px;
            height:400px;
            opacity: 1;
      }
    }
    @keyframes middleMainMobile {
      0%{
            width: 150px;
            height: 220px;
            left: 0;
            z-index: 3;
            opacity: 0.8;
      }
      100%{
            left: 18%;
            width:80%;
            height:80%;
            opacity: 1;
      }
    }
}
.img-main-right{
    transition: 0.5s ease-in-out;
    animation: middleMainRight  1s 0s forwards;
      @include for-mobile{
      display: block;
      }
      img{
        width: 100%;
        height: 100%;
        border-radius: 4%;
        opacity: 0.8;
        @include for-mobile{
        width: 65%;
        }
      }

    @keyframes middleMainRight {

      0%{
            width: 150px;
            height: 220px;
            margin-left: 70%;
            z-index: 3;
            opacity: 1;

      }
      100%{
            margin-left: 23.5%;
            width:300px;
            height:400px;

      }
    }
}
.img-main{
    width:150px;
    height:200px;
    margin-left: 13%;
    z-index: 3;
  @include for-mobile{
      display: block;
      margin-left: 0%;
  }
  @include for-desktop{
img{
              width:100%;
              height:100%;
              border-radius: 4%;
        }
  }
}
  .img-disappear{
  display: none;
      img{
          width: 150px;
          height: 150px;
      }
  }
  }
  .icon-left,.icon-right{
  top: var(--spacer-3xl);
  background: none;
  cursor: pointer;
  width: 6%;

      img{
        background:var(--_c-paige-primary);
        width: 2rem;
        height: 2rem;
        border-radius: 25%;
        padding-right: 0.3rem;
        padding-left: 0.3rem;
        &:hover{
          background: var(--_c-braun-primary);
        }
      }
  }
    .icon-left{
      left: 0;
      padding-top: 271px;
      @include for-mobile{
        padding-top: 171px;
      }
    }
    .icon-right{
      right: 0;
      padding-top: 271px;
     @include for-mobile{
        padding-top: 171px;
      }
    }
}
//Start related Product Hover
.img-main-left,.img-main-right{
  img{
    transition: 0.4s ease-in-out;
  }
.related-product-price{
 transition: 0.4s ease-in-out;
  }
  &:hover{
    img{
   box-shadow: 0px 1px 6px 6px var(--_c-paige-primary);
    }
    .related-product-price{

     transform: translateY(1rem);
      color: var(--c-white);
    }
  }
}
//End related Product Hover
</style>
