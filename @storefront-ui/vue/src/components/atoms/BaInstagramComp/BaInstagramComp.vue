<template>
<div class="ba-instagram">
  <div class="title-container">
    <h2>{{$t("Share Your Look")}}</h2>
    <h4>#YOURLOOK</h4>
  </div>

  <div class="ba-Instagram-comp">
    <div class="box1">
      <img class="img-container" src="/homepage/imageAd.webp" alt="katherina_trn">
      <p class="container-text">katherina_trn</p>
    </div>
    <div class="box1">
      <img class="img-container" src="/homepage/imageAd.webp" alt="katherina_trn">
      <p class="container-text">katherina_trn</p>
    </div>
    <div class="box1">
      <img class="img-container" src="/homepage/imageDd.webp" alt="katherina_trn">
      <p class="container-text">katherina_trn</p>
    </div>
    <div class="box1">
      <img class="img-container" src="/homepage/imageAd.webp" alt="katherina_trn">
      <p class="container-text">katherina_trn</p>
    </div>
  </div>
</div>

</template>
<script>
import SfImage from "@storefront-ui/vue/src/components/atoms/SfImage/SfImage.vue";
import {mapMobileObserver,unMapMobileObserver,} from "@storefront-ui/vue/src/utilities/mobile-observer.js";
export default {
  name:"BaInstagramComp",

  components: {

    SfImage,
  },
  computed: {
    ...mapMobileObserver(),
  },
  beforeDestroy() {
    unMapMobileObserver();
  },
}
</script>
<style lang="scss" >
.ba-instagram{
  width: 100%;
  max-height: 150rem;
  margin-bottom: 15rem;

.title-container{
    width: 100%;
    max-height: 150rem;
    text-align: center;
   h2{ font-family: var(--font-family--title) ;letter-spacing: 2px;
   }
  }
.ba-Instagram-comp{
  width: 100%;
  max-height: 150rem;
  margin-bottom: 15rem;

  display: flex;
  flex-direction: row;
  justify-content: center;
  transform-origin: center;
  margin-top: var(--spacer-xl);
  margin-bottom: var(--spacer-xl);
  .box1{
    position: relative;
    width:18%;
    height: 90%;
    left: 0;
    transition: 0.4s ease-in-out;
    cursor: pointer;

    img{
      width: 100%;
      height: 80%;
      border-radius: 50%;
      transform: perspective(800px) rotateY(60deg);
       z-index: 2;
      transition: 0.3s ease-in-out;


    }
    .container-text{
      position: absolute;
      transition: 0.5s ease-in-out;
      top: 0;
      font-size: 0px;
      z-index: -1;
      left: 40%;

    }
 @include for-desktop {
  &:hover{
   width:22%;
    img{
      width: 80%;
      height: 75%;
      transform: perspective(800px) rotateY(0);
      box-shadow: 0px 4px 20px 4px var(--_c-paige-primary);


    }
    .container-text{
      top:calc(var(--spacer-3xl) + var(--spacer-2xl));
      font-size: 16px;
      left: 23%;
    }
  }

 }


  }

}
  @include for-mobile {
    .ba-Instagram-comp{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top:var(--spacer-2xl);

     .box1{
       width:50%;
       img{
         transform: perspective(0) rotateY(0);
       }
     }
   }
  }
 }
</style>
