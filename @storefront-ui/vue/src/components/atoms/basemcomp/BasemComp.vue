<template>
  <div class="basem-comp">
      <div class="offer-field">
                    <div  v-for="offer in offers" :key="offer.key"  class="box box1">
                          <SfImage class="img-container" :alt="offer.offeralt" :src="offer.offerImgSrc" :width="116" :height="100"></SfImage>
                       <div class="comp-text">
                           <h4 >{{$t(offer.offerTitle)}}</h4>
                            <h5 >{{$t(offer.offerText)}}</h5>

                        </div>

                    </div>



      </div>
  </div>
</template>

<script>
import SfCarousel from "@storefront-ui/vue/src/components/organisms/SfCarousel/SfCarousel.vue";
import {SfArrow,SfCarouselItem,SfCard,SfImage} from '@storefront-ui/vue';
export default {
  name:"BasemComp",
  components:{
    SfCarousel,
    SfArrow,
    SfCarouselItem,
    SfCard,
    SfImage
  },
  data(){
    return {
      offers:[
        {
      offerTitle:"Offer Title",
      offerText:"Offer Is Here",
      offeralt:"Offer Img",
      offerImgSrc:"https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/080175_1_large.jpg",
        },
        {
      offerTitle:"Offer Title",
      offerText:"Offer Is Here",
      offeralt:"Offer Img",
      offerImgSrc:"https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/079922_1_large.jpg",
        },
        {
      offerTitle:"Offer Title",
      offerText:"Offer Is Here",
      offeralt:"Offer Img",
      offerImgSrc:"https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072750_1_large.jpg",
        },
      ],
    };
  },
};
</script>
<style lang="scss">
.basem-comp{
  height:  20rem;
  width: 98.5% !important;
  color: var(--_c-dark-primary);
  //background: red;
    .offer-field{
        width: 100%;
        height: 90%;
        box-sizing: border-box;
        display: flex;
        overflow: hidden;
        justify-content: space-around;
        background: linear-gradient(to bottom,var(--_c-braun-primary),var(--_c-paige-primary));
        padding-bottom:var(--spacer-2xs);
        padding-top:(--spacer-2xs);
        border-bottom-right-radius: 5%;
        border-bottom-left-radius: 5%;
            .box{
              width: 18rem;
              height: 85%;
              background: var(--c-white);
              margin-top: var(--spacer-xs);
              padding: var(--spacer-xs);
              box-sizing: border-box;
              display: flex;
              justify-content: space-between;
              cursor: pointer;
              padding-top: var(--spacer-base);
              border-radius: 2%;
              box-shadow: 2px 2px 5px 5px var(--_c-dark-primary);
              &:hover{
                transition: 0.4s ease-in-out;
                margin-top: var(--spacer-sm);
                box-shadow: 1px 1px 2px 2px var(--_c-dark-primary);
              }
              img{
                background: var(--c-light);
                float: left;
                border-radius: 2%;
              }
              .comp-text{
                width:calc(var(--spacer-2xl) + var(--spacer-2xl));
                height:var(--spacer-2xl);
                text-align: center;
                h4{
                  text-transform: uppercase;
                }
              }
             }
    }
}
</style>
