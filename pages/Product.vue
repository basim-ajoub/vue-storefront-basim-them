<template>
  <div   id="product">

 <SfBreadcrumbs
      class="breadcrumbs desktop-only"
      :breadcrumbs="breadcrumbs"
    />

  <div  id="product-comp"  class="product">
      <div class="test">


  <!--<LazyHydrate when-idle>
        <SfGallery :images="productGallery" class="product__gallery" />
      </LazyHydrate>--->

<!--Satrt Add Custome Component to display the Product--<LazyHydrate when-idle>

    </LazyHydrate>-->
<BaProductCollection :images="productGallery" class="product__gallery" />
    <!--End Add Custome Component to display the Product-->
    <!--mobile display-->
      <div  id="product__info" class="product__info">
        <div id="product-header-price" class="product-header-price">
          <div class="product__header">
            <SfHeading
              :title="productGetters.getName(product)"
              :level="3"
              class="sf-heading--no-underline sf-heading--left"
            />
            <SfIcon
              icon="drag"
              size="xxl"
              color="var(--c-text-disabled)"
              class="product__drag-icon smartphone-only"
            />
          </div>
          <div class="product__price">
            <SfPrice
              :regular="$n(productGetters.getPrice(product).regular, 'currency')"
              :special="productGetters.getPrice(product).special && $n(productGetters.getPrice(product).special, 'currency')"
            />
          </div>
        </div>
         <div class="sale">40%</div>
        <div class="infos">
          <p class="product__description desktop-only">
            {{ description }}
          </p>
          <SfButton class="sf-button--text desktop-only product__guide">
            {{ $t('Size guide') }}
          </SfButton>
          <div class="product-size-sel-and-express-label">


          <SfSelect
            v-e2e="'size-select'"
            v-if="options.size"
            :value="configuration.size"
            @input="size => updateFilter({ size })"
            label="Size"
            class="sf-select--underlined product__select-size"
            :required="true"
          >
            <SfSelectOption
              v-for="size in options.size"
              :key="size.value"
              :value="size.value"
            >
              {{size.label}}
            </SfSelectOption>
          </SfSelect>
          <div class="express">{{$t("Express")}}</div>
        </div>
          <div v-if="options.color && options.color.length > 1" class="product__colors desktop-only">
            <p class="product__color-label">{{ $t('Color') }}:</p>
            <SfColor
              v-for="(color, i) in options.color"
              :key="i"
              :color="color.value"
              class="product__color"
              @click="updateFilter({ color: color.value })"
            />

          </div>


          <div
            class="product__delivery"
            v-if="channels.length > 0"
          >
            <SfRadio
              v-e2e="'delivery-option'"
              name="Delivery"
              :label="$t('Delivery')"
              value="delivery"
              :selected="selectedDelivery"
              @input="setSelectedDelivery('delivery')"
            />
            <SfRadio
              v-e2e="'click-collect-option'"
              name="Click & Collect"
              :label="$t('Pickup in the store')"
              :details="$t('Free')"
              value="collect"
              :selected="selectedDelivery"
              @input="setSelectedDelivery('collect')"
            />
            <SfSelect
              v-if="selectedDelivery === 'collect'"
              v-e2e="'channel-select'"
              v-model="channelId"
              :label="$t('Select Channel')"
              class="sf-select--underlined product__select-size"
            >
              <SfSelectOption
                v-for="{ channel } in channels"
                :key="channel.id"
                :value="channel.id"
              >
                {{channel.name}}
              </SfSelectOption>
            </SfSelect>
          </div>

          <SfAddToCart
            v-e2e="'product_add-to-cart'"
            :stock="stock"
            v-model="qty"
            :disabled="loading"
            :canAddToCart="stock > 0"
            class="product__add-to-cart"
            @click="addToCart"
          />
        </div>
    <div class="product__price-and-rating">
          <div>
            <div class="product__rating">
              <SfRating
                :score="averageRating"
                :max="5"
              />
              <a v-if="!!totalReviews" href="#" class="product__count">
                ({{ totalReviews }})
              </a>
            </div>
            <SfButton class="sf-button--text">{{ $t('Read all reviews') }}</SfButton>
          </div>
        </div>
        <LazyHydrate when-idle>
          <SfTabs :open-tab="1" class="product__tabs">
            <SfTab title="Description">
              <div class="product__description">
                {{ $t('Product description') }}
              </div>
              <SfProperty
                v-for="(property, i) in properties"
                :key="i"
                :name="property.name"
                :value="property.value"
                class="product__property"
              >
                <template v-if="property.name === 'Category'" #value>
                  <SfButton class="product__property__button sf-button--text">
                    {{ property.value }}
                  </SfButton>
                </template>
              </SfProperty>
            </SfTab>
            <SfTab title="Read reviews">
              <SfReview
                v-for="review in reviews"
                :key="reviewGetters.getReviewId(review)"
                :author="reviewGetters.getReviewAuthor(review)"
                :date="reviewGetters.getReviewDate(review)"
                :message="reviewGetters.getReviewMessage(review)"
                :max-rating="5"
                :rating="reviewGetters.getReviewRating(review)"
                :char-limit="250"
                read-more-text="Read more"
                hide-full-text="Read less"
                class="product__review"
              />
            </SfTab>
            <SfTab
              title="Additional Information"
              class="product__additional-info"
            >
              <div class="product__additional-info">
                <p class="product__additional-info__title">{{ $t('Brand') }}</p>
                <p>{{ brand }}</p>
                <p class="product__additional-info__title">{{ $t('Instruction1') }}</p>
                <p class="product__additional-info__paragraph">
                  {{ $t('Instruction2') }}
                </p>
                <p class="product__additional-info__paragraph">
                  {{ $t('Instruction3') }}
                </p>
                <p>{{ careInstructions }}</p>
              </div>
            </SfTab>
          </SfTabs>
        </LazyHydrate>
      </div>
    </div>
  </div>
    <LazyHydrate when-visible>
        <!--Start Image Maximize-->
      <div class="image-window">
        <h2 class="title-match-product">{{$t("Match It With")}}</h2>
         <BaProductShow :products="relatedProducts" :loading="relatedLoading"   class="product__maximize" />
      </div>
       <!--End Image Maximize-->
      <!--<RelatedProducts
        :products="relatedProducts"
        :loading="relatedLoading"
        title="Match it with"
      />-->
    </LazyHydrate>

    <LazyHydrate when-visible>
      <BaInstagramComp />
    </LazyHydrate>

  </div>
</template>
<script>
import {
  SfProperty,
  SfHeading,
  SfPrice,
  SfRating,
  SfRadio,
  SfSelect,
  SfAddToCart,
  SfTabs,
  SfGallery,
  SfIcon,
  SfImage,
  SfBanner,
  SfAlert,
  SfSticky,
  SfReview,
  SfBreadcrumbs,
  SfButton,
  SfColor,


} from '@storefront-ui/vue';
import BaProductShow from '../@storefront-ui/vue/src/components/atoms/BaImgShow/BaImgShow.vue';
import BaProductCollection from '../@storefront-ui/vue/src/components/atoms/BaproductCollection/BaProductCollection.vue';
//import BaInstagramComp from '@storefront-ui/vue/src/components/atoms/BaInstagramComp/BaInstagramComp.vue';
import BaInstagramComp from '../@storefront-ui/vue/src/components/atoms/BaInstagramComp/BaInstagramComp.vue';
import InstagramFeed from '~/components/InstagramFeed.vue';
import RelatedProducts from '~/components/RelatedProducts.vue';
import { ref, computed, useRoute, useRouter } from '@nuxtjs/composition-api';
import {
  useProduct,
  useCart,
  productGetters,
  useReview,
  reviewGetters,
  useStore
} from '@vue-storefront/commercetools';
import { onSSR } from '@vue-storefront/core';
import LazyHydrate from 'vue-lazy-hydration';
import cacheControl from './../helpers/cacheControl';

export default {
  name: 'Product',
  transition: 'fade',
  middleware: cacheControl({
    'max-age': 60,
    'stale-when-revalidate': 5
  }),
  setup() {
    const qty = ref(1);
    const route = useRoute();
    const router = useRouter();
    const { products, search } = useProduct('products');
    const { products: relatedProducts, search: searchRelatedProducts, loading: relatedLoading } = useProduct('relatedProducts');
    const { addItem, loading } = useCart();
    const { reviews: productReviews, search: searchReviews } = useReview('productReviews');
    const { response: stores } = useStore();

    // to be added on local useStore factory
    function getSelected(stores) {
      return stores.results?.find((result) => result.key === stores._selectedStore);
    }

    const product = computed(() => productGetters.getFiltered(products.value, { master: true, attributes: route.value.query })[0]);
    const options = computed(() => productGetters.getAttributes(products.value, ['color', 'size']));
    const configuration = computed(() => productGetters.getAttributes(product.value, ['color', 'size']));
    const categories = computed(() => productGetters.getCategoryIds(product.value));
    const reviews = computed(() => reviewGetters.getItems(productReviews.value));
    const selectedStore = computed(() => getSelected(stores.value));

    const channelId = ref(null);
    const channels = computed(() => {
      const productChannels = product.value?.availability?.channels?.results ?? [];
      return productChannels;
    });
    const selectedDelivery = ref(null);
    const setSelectedDelivery = option => selectedDelivery.value = option;

    const selectedChannel = computed(() => {
      if (selectedDelivery.value !== 'collect') return null;
      const selected = channels.value.find((item) => (item.channel.id === channelId.value));

      return (selected?.channel?.roles && selected?.channel?.id) ? {
        ...(selected.channel.roles.includes('InventorySupply') && { supplyChannel: selected.channel.id }),
        ...(selected.channel.roles.includes('ProductDistribution') && { distributionChannel: selected.channel.id })
      } : null;
    });

    const addToCart = () => {
      addItem({
        product: {
          id: product.value.id,
          sku: product.value.sku
        },
        quantity: parseInt(qty.value),
        customQuery: selectedChannel.value
      });
    };

    // TODO: Breadcrumbs are temporary disabled because productGetters return undefined. We have a mocks in data
    // const breadcrumbs = computed(() => productGetters.getBreadcrumbs ? productGetters.getBreadcrumbs(product.value) : props.fallbackBreadcrumbs);
    const productGallery = computed(() => productGetters.getGallery(product.value).map(img => ({
      mobile: { url: img.small },
      desktop: { url: img.normal },
      big: { url: img.big },
      alt: product.value._name || product.value.name
    })));

    onSSR(async () => {
      await search({ id: route.value.params.id });
      await searchRelatedProducts({ catId: [categories.value[0]], limit: 8 });
      await searchReviews({ productId: route.value.params.id });
    });

    const updateFilter = (filter) => {
      router.push({
        path: route.value.path,
        query: {
          ...configuration.value,
          ...filter
        }
      });
    };

    return {
      addToCart,
      updateFilter,
      configuration,
      product,
      products,
      reviews,
      reviewGetters,
      averageRating: computed(() => productGetters.getAverageRating(product.value)),
      totalReviews: computed(() => productGetters.getTotalReviews(product.value)),
      relatedProducts: computed(() => productGetters.getFiltered(relatedProducts.value, { master: true })),
      relatedLoading,
      options,
      qty,
      loading,
      productGetters,
      productGallery,
      channels,
      channelId,
      selectedChannel,
      selectedStore,
      selectedDelivery,
      setSelectedDelivery
    };
  },
  components: {
    SfAlert,
    SfColor,
    SfProperty,
    SfHeading,
    SfPrice,
    SfRating,
    SfRadio,
    SfSelect,
    SfAddToCart,
    SfTabs,
    SfGallery,
    SfIcon,
    SfImage,
    SfBanner,
    SfSticky,
    SfReview,
    SfBreadcrumbs,
    SfButton,
    InstagramFeed,
    RelatedProducts,
    LazyHydrate,
    BaProductShow,
    BaProductCollection,
    BaInstagramComp,

  },
  methods:{
    test(){




        window.onscroll = () => {
          var x = screen.width;
          if(x > 1000){
          //  window.alert(x);
            var y = window.scrollY;
            var productWindow = document.getElementById("product-comp");
            var productWindowHeight = productWindow.offsetHeight;
            var productWindowtop = productWindow.offsetTop;

            if(y>productWindowtop && y<productWindowHeight - 800){//if scrolling is on product component
                  // x.scrollIntoView();//to jump to the product view
                  //document.body.style.overflowY = "hidden";
                  //x.style.overflowY = "auto";
                  // window.alert(y);
                  document.getElementById("product__info").style.position = "fixed";
                  document.getElementById("product__info").style.right = "4rem";
                  document.getElementById("product__info").style.top = "1rem";
                  //  window.scrollTo(0,200);//scroll o top of the product
                  //x.focus();

            }
            else if(y>productWindowHeight - 800){
              document.getElementById("product__info").style.position = "static";
              document.getElementById("product__info").style.marginTop = "auto";


            }
            else{
                  document.getElementById("product__info").style.position = "initial";
                  document.getElementById("product__info").style.marginTop = "0rem";
                  //window.alert(x.scrollHeight);
                  // window.alert(y);
            }
          }
          else{
             document.getElementById("product__info").style.position = "initial";
            }
        }

   },
},


 mounted () {
      this.test();
 },

  data() {
    return {
      stock: 5,
      scroll:0,
      properties: [
        {
          name: 'Product Code',
          value: '578902-00'
        },
        {
          name: 'Category',
          value: 'Pants'
        },
        {
          name: 'Material',
          value: 'Cotton'
        },
        {
          name: 'Country',
          value: 'Germany'
        }
      ],
      description: 'Find stunning women cocktail and party dresses. Stand out in lace and metallic cocktail dresses and party dresses from all your favorite brands.',
      detailsIsActive: false,
      brand:
        'Brand name is the perfect pairing of quality and design. This label creates major everyday vibes with its collection of modern brooches, silver and gold jewellery, or clips it back with hair accessories in geo styles.',
      careInstructions: 'Do not wash!',
      breadcrumbs: [
        {
          text: 'Home',
          route: {
            link: '#'
          }
        },
        {
          text: 'Category',
          route: {
            link: '#'
          }
        },
        {
          text: 'Pants',
          route: {
            link: '#'
          }
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>
//Start test

.test{
  width: 100%;
 // background: red;
 @include for-desktop{
  display: flex;
  flex-direction: row;
 }

}
//End test
#product {
  box-sizing: border-box;
  //font-family: 'Unna', serif;
  @include for-desktop {
    max-width: 1272px;
    margin: 0 auto;
    //Start Change The product window
    margin-top: 5rem;
    //End Change The product window


  }

}
//Start The Product Collection Class
//Start Change in header
.product-header-price{
  width: 100%;
  display: flex;
  flex-direction: row;


 .product__header{
    width:100%;
    height: 5rem;
    }//End Change in header

.product__price{
  padding: var(--spacer-xs);

  }

} .sale{//Start Change for Sale field
max-width: 9%;
height: var(--spacer-base);
border-radius: 50%;
text-align: center;
background: linear-gradient(to top ,red ,rgba(165, 18, 18, 0.767));
color:var(--c-white);
padding: var(--spacer-2xs);
@include for-mobile{
  margin-top: var(--spacer-base);
  margin-bottom: var(--spacer-base);
  font-size: var(--font-size--xs);

}
  }//End Change for Sale field
//End The Product Collection Class
.product {
  //Start Change Product
  background: var(--c-white);
  font-family: var(--font-family--text-containe);
  padding:  var(--spacer-lg);
  @include for-desktop{
    margin-top: -15px;
  }
 //End Change Product

  @include for-desktop {
    display: flex;
  }
  &__info {
    //change width for mobile
    //max-width: 32.625rem;
    max-width: 48%;
    margin: var(--spacer-sm) auto;
    @include for-mobile{
        max-width: 100%;
        position: initial;
    }
    //Start CHange

    //End Change

    @include for-desktop {
      position: initial;
      max-width: 32.625rem;
      margin: 0 0 0 7.5rem;
    }
    //Start Change
    .infos{
      .product-size-sel-and-express-label{
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
      .express{
        background: linear-gradient(to top ,red ,rgba(165, 18, 18, 0.767));
        width: 12%;
        height: var(--spacer-base);
        padding: 0.25rem;
        border-top-right-radius: 20%;
        border-bottom-left-radius: 20%;
        border-bottom-right-radius: 20%;
        color: var(--c-white);
        text-align: center;
        font-family: var(--font-family--title);
        letter-spacing: 1px;
        @include for-mobile{
          font-size: var(--font-size--xs);
        }
      }
      }
    }
     //End Change
  }
  &__header {
    --heading-title-color: var(--c-link);
    --heading-title-font-weight: var(--font-weight--bold);
    --heading-padding: 0;
    margin: 0 var(--spacer-sm);
    display: flex;
    justify-content: space-between;
    //Start Change in Font Style "title"
    @include for-desktop {
      --heading-title-font-weight: var(--font-weight--semibold);
      --heading-title-font-size:var(--font-size--xl);
      margin: 0 auto;
    }
    //End Change in Font Style "title"
  }
  &__drag-icon {
    animation: moveicon 1s ease-in-out infinite;
  }

  &__price-and-rating {
    margin: 0 var(--spacer-sm) var(--spacer-base);
    align-items: center;

    @include for-desktop {
      display: flex;
      justify-content: space-between;
      margin: var(--spacer-sm) 0 var(--spacer-lg) 0;
    }
  }
  &__rating {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin: var(--spacer-xs) 0 var(--spacer-xs);
  }
  &__count {

    @include font(
        --count-font,
        var(--font-weight--normal),
        var(--font-size--sm),
        1.4,
        var(--font-family--secondary)
    );
    color: var(--c-text);
    text-decoration: none;
    margin: 0 0 0 var(--spacer-xs);
  }
  &__description {
    //Start Change
color: var(--_c-dark-secondary);
    //End Change
    @include font(
        --product-description-font,
        var(--font-weight--light),
        var(--font-size--base),
        1.6,
        var(--font-family--primary)
    );
  }

  &__select-size {
    margin: 0 var(--spacer-sm);
    //Start Change The width of size input
    width:20%;
    //End Change The width of size input


    @include for-desktop {
      margin: 0;
    }
  }
  &__colors {
    @include font(
        --product-color-font,
        var(--font-weight--normal),
        var(--font-size--lg),
        1.6,
        var(--font-family--secondary)
    );
    display: flex;
    align-items: center;
    margin-top: var(--spacer-xl);
  }
  &__color-label {
    margin: 0 var(--spacer-lg) 0 0;

  }
  &__color {
    margin: 0 var(--spacer-2xs);
  }
  &__add-to-cart {
    margin: var(--spacer-base) var(--spacer-sm) 0;
    @include for-desktop {
      margin-top: var(--spacer-2xl);
    }
  }
  &__guide,
  &__compare,
  &__save {
    display: block;
    margin: var(--spacer-xl) 0 var(--spacer-base) auto;
  }
  &__compare {
    margin-top: 0;
  }
  &__tabs {
    --tabs-title-z-index: 0;
    margin: var(--spacer-lg) auto var(--spacer-2xl);
    --tabs-title-font-size: var(--font-size--lg);
    @include for-desktop {
      margin-top: var(--spacer-2xl);
    }
  }
  &__property {
    margin: var(--spacer-base) 0;
    &__button {
      --button-font-size: var(--font-size--base);
    }
  }
  &__review {
    padding-bottom: 24px;
    border-bottom: var(--c-light) solid 1px;
    margin-bottom: var(--spacer-base);
  }
  &__additional-info {
    color: var(--c-link);
    @include font(
        --additional-info-font,
        var(--font-weight--light),
        var(--font-size--sm),
        1.6,
        var(--font-family--primary)
    );
    &__title {
      font-weight: var(--font-weight--normal);
      font-size: var(--font-size--base);
      margin: 0 0 var(--spacer-sm);
      &:not(:first-child) {
        margin-top: 3.5rem;
      }
    }
    &__paragraph {
      margin: 0;
    }
  }
  &__gallery {
    flex: 1;
  }
}
.breadcrumbs {
//Start Change
  margin: var(--spacer-base) auto 0;
  background:var(--_c-braun-primary);
//End Change
}
@keyframes moveicon {
  0% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, 30%, 0);
  }
  100% {
    transform: translate3d(0, 0, 0);
  }
}
//Start Change in add button only
.product__add-to-cart{
  --button-border-radius:17%;//edit file sfaddtocart.scss
}
  .product__add-to-cart:hover{
  --button-add-background:linear-gradient(to right,var(--_c-braun-secondary),var(--_c-braun-primary)) !important;//edit file sfaddtocart.scss
  }//End Change in add button only
  .product__price-and-rating{//Start change position for rating component
    display: flex;
    justify-content: right;

  }//End change position for rating component
//Start Image Maximize effect
.image-window{
  width: 100%;
  margin-top: var(--spacer-2xl);
  margin-bottom: var(--spacer-3xl);
text-align: center;
  .title-match-product{
  margin-bottom: 5rem;
  font-family: var(--font-family--title);
  letter-spacing: 1px;
  }
}
.product__description{
  font-family: var(--font-family--text-containe);
  letter-spacing: 0.5px;
}
//End Image Maximize effect
</style>
