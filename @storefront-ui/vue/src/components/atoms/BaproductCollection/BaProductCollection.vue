<template>
  <div class="product-collection-comp">
    <div v-for="(picture,index) in images" :key="'slide-' + index" id="container" class="container">

      <div class="box img-1">
                <SfImage class="sf-product" :src="picture.desktop.url" :alt="picture.alt" width="370" height="490" />

      </div>
      <div class="box img-1">
                <SfImage class="sf-product" :src="picture.desktop.url" :alt="picture.alt"  width="370" height="490" />

      </div>
      <div class="box img-1">
                <SfImage class="sf-product" :src="picture.desktop.url" :alt="picture.alt" width="370" height="490" />

      </div>
          <div class="box img-1">
                <SfImage class="sf-product" :src="picture.desktop.url" :alt="picture.alt" width="370" height="490" />

      </div>
    </div>
  </div>
</template>

<script>
import SfImage from "@storefront-ui/vue/src/components/atoms/SfImage/SfImage.vue";
export default {
name:"productCollectionComp",
components:{
  SfImage,
},
props: {
    /**
     * Images list
     */
    images: {
      type: Array,
      default: () => [],
    },
},
}
</script>
<style lang="scss" scoped>
.product-collection-comp{
  max-width: 50%;
  max-height: 200rem;
  box-sizing: border-box;
  overflow: hidden;
  @include for-mobile{
    max-width:100%;
  }
  .container{
    width: 80%;
    max-height: 200rem;
    display: flex;
    overflow-x: hidden ;
    flex-direction: column;
    place-items: left;
    margin-top:var(--spacer-lg);
    @include  for-desktop{
      place-items: center;
      padding: var(--spacer-lg);
    }
    @include for-mobile{
      width: 100%;
    }
        .box{
        width: 70%;
        height: 100%;
        margin-bottom:calc(var(--spacer-lg) + var(--spacer-xl));
        cursor: pointer;
          .sf-product{
              width: 100%;
                height: 100%;
              img{
                  width: 100%;
                  height: 100%;
              }
          }
    }
  }
}
</style>
