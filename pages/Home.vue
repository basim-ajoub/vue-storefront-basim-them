<template>
  <div id="home">
    <LazyHydrate when-idle>
      <SfHero class="hero">
        <SfHeroItem
          v-for="(hero, i) in heroes"
          :key="i"
          :title="hero.title"
          :subtitle="hero.subtitle"
          :background="hero.background"
          :image="hero.image"
          :class="hero.className"
        />
      </SfHero>
    </LazyHydrate>

    <LazyHydrate when-visible>
      <SfBannerGrid :banner-grid="1" class="banner-grid">
        <template v-for="item in banners" v-slot:[item.slot]>
          <SfBanner
            :key="item.slot"
            :title="item.title"
            :subtitle="item.subtitle"
            :description="item.description"
            :button-text="item.buttonText"
            :link="localePath(item.link)"
            :image="item.image"
            :class="item.class"
          />
        </template>
      </SfBannerGrid>
    </LazyHydrate>

    <LazyHydrate when-visible>
      <div class="similar-products">
        <SfHeading title="Match with it" :level="2" />
        <nuxt-link :to="localePath('/c/women')" class="smartphone-only">
          {{ $t("See all") }}
        </nuxt-link>
      </div>
    </LazyHydrate>
    <!--Start Women Products-->
    <LazyHydrate when-visible>
      <SfCarousel
        class="carousel"
        :settings="{ peek: 16, breakpoints: { 1023: { peek: 0, perView: 2 } } }"
      >
        <template #prev="{ go }">
          <SfArrow
            aria-label="prev"
            class="sf-arrow--left sf-arrow--long"
            @click="go('prev')"
          />
        </template>
        <template #next="{ go }">
          <SfArrow
            aria-label="next"
            class="sf-arrow--right sf-arrow--long"
            @click="go('next')"
          />
        </template>
        <SfCarouselItem
          class="carousel__item"
          v-for="(product, i) in products"
          :key="i"
        >
          <SfProductCard
            :title="product.title"
            :image="product.image"
            :regular-price="product.price.regular"
            :max-rating="product.rating.max"
            :score-rating="product.rating.score"
            :show-add-to-cart-button="true"
            :is-on-wishlist="product.isInWishlist"
            :link="localePath({ name: 'home' })"
            class="carousel__item__product"
            @click:wishlist="toggleWishlist(i)"
          />
        </SfCarouselItem>
      </SfCarousel>
    </LazyHydrate>
    <!--End Women Products-->

    <!--End Offers Blocks-->

    <div class="type-store">
      <BaDisscount
        v-for="type in typeitems"
        :key="type.key"
        :image="type.image"
        :cardtitle="type.title"
        :carddescription="type.description"
        :cardcategory="type.category"
      />
    </div>

    <!--Start Offers Blocks-->
    <LazyHydrate v-for="offer in offers" :key="offer.key" when-visible>
      <SfCallToAction
        :title="offer.title"
        button-text="Subscribe"
        :description="offer.description"
        :image="offer.image"
        class="call-to-action newsletter-div p-section"
      >
        <template #button>
          <SfButton
            class="sf-call-to-action__button"
            data-testid="cta-button"
            @click="toggleNewsletterModal"
          >
            {{ $t("Subscribe") }}
          </SfButton>
        </template>
      </SfCallToAction>
    </LazyHydrate>

    <LazyHydrate when-visible>
      <NewsletterModal @email-submitted="onSubscribe" />
    </LazyHydrate>

    <LazyHydrate when-visible>
      <InstagramFeed />
    </LazyHydrate>
  </div>
</template>
<script>
import {
  SfHero,
  SfBanner,
  SfCallToAction,
  SfSection,
  SfCarousel,
  SfProductCard,
  SfImage,
  SfBannerGrid,
  SfHeading,
  SfArrow,
  SfCard,
  SfButton,
} from "@storefront-ui/vue";
import { ref, useContext } from "@nuxtjs/composition-api";
import InstagramFeed from "~/components/InstagramFeed.vue";
import NewsletterModal from "~/components/NewsletterModal.vue";
import LazyHydrate from "vue-lazy-hydration";
import { useUiState } from "../composables";
import cacheControl from "./../helpers/cacheControl";
import BaDisscount from "../@storefront-ui/vue/src/components/molecules/BaDisscount/BaDisscount.vue";

export default {
  name: "Home",
  middleware: cacheControl({
    "max-age": 60,
    "stale-when-revalidate": 5,
  }),
  components: {
    InstagramFeed,
    SfHero,
    SfBanner,
    SfCallToAction,
    SfSection,
    SfCarousel,
    SfProductCard,
    SfImage,
    SfBannerGrid,
    SfHeading,
    SfArrow,
    SfButton,
    NewsletterModal,
    LazyHydrate,
    SfCard,
    BaDisscount,
  },
  setup() {
    const { $config } = useContext();
    const { toggleNewsletterModal } = useUiState();
    const typeitems = ref([
      {
        title: "Cocktail & Party",
        description: "Basim Discription",
        category: "Dress",
        image: "/homepage/bannerB.webp",
      },
      {
        title: "Linen Dresses",
        description:
          "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands    ",
        category: "Dress",
        image: "/homepage/bannerE.webp",
      },
      {
        title: "The Office Life",
        description:
          "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands    ",
        category: "T-Shirts",
        image: "/homepage/bannerC.webp",
      },
      {
        title: "Cocktail & Party",
        description:
          "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands    ",
        category: "Summer Sandals",
        image: "/homepage/bannerG.webp",
      },
    ]);
    const offers = ref([
      {
        title: "basim title",
        description: "Basim Discription",
        image: "/homepage/newsletter.webp",
      },
      {
        title: "basim title",
        description: "Basimm Discription",
        image: "/homepage/productB.webp",
      },
      {
        title: "basim title",
        description: "Basimmm Discription",
        image: "/homepage/newsletter.webp",
      },
    ]);
    const products = ref([
      {
        title: "Cream Beach Bag",
        image: "/homepage/productA.webp",
        price: { regular: "10.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: true,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productB.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productC.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productA.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productB.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productC.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productA.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image: "/homepage/productB.webp",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 3 },
        isInWishlist: false,
      },
    ]);
    const menproducts = ref([
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: true,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 4 },
        isInWishlist: false,
      },
      {
        title: "Cream Beach Bag",
        image:
          "https://s3-eu-west-1.amazonaws.com/commercetools-maximilian/products/072703_1_large.jpg",
        price: { regular: "50.00 $" },
        rating: { max: 5, score: 3 },
        isInWishlist: false,
      },
    ]);
    const heroes = [
      {
        title: "Colorful summer dresses are already in store",
        subtitle: "SUMMER COLLECTION 2019",
        background: "#eceff1",
        image: "/homepage/bannerH.webp",
      },
      {
        title: "Colorful summer dresses are already in store",
        subtitle: "SUMMER COLLECTION 2019",
        background: "#efebe9",
        image: "/homepage/bannerA.webp",
        className:
          "sf-hero-item--position-bg-top-left sf-hero-item--align-right",
      },
      {
        title: "Colorful summer dresses are already in store",
        subtitle: "SUMMER COLLECTION 2019",
        background: "#fce4ec",
        image: "/homepage/bannerB.webp",
      },
    ];
    const banners = [
      {
        slot: "banner-A",
        subtitle: "Dresses",
        title: "Cocktail & Party",
        description:
          "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands.",
        buttonText: "Shop Now",
        image: {
          mobile: $config.theme.home.bannerA.image.mobile,
          desktop: $config.theme.home.bannerA.image.desktop,
        },
        class: "sf-banner--slim sf-banner-left desktop-only",
        link: $config.theme.home.bannerA.link,
      },
      {
        slot: "banner-B",
        subtitle: "Dresses",
        title: "Linen Dresses",
        description:
          "Find stunning women's cocktail dresses and party dresses. Stand out in lace and metallic cocktail dresses from all your favorite brands.",
        buttonText: "Shop now",
        image: $config.theme.home.bannerB.image,
        class: "sf-banner--slim banner-central desktop-only",
        link: $config.theme.home.bannerB.link,
      },
      {
        slot: "banner-C",
        subtitle: "T-Shirts",
        title: "The Office Life",
        image: $config.theme.home.bannerC.image,
        class: "sf-banner--slim sf-banner-right-top banner__tshirt",
        link: $config.theme.home.bannerC.link,
      },
      {
        slot: "banner-D",
        subtitle: "Summer Sandals",
        title: "Eco Sandals",
        image: $config.theme.home.bannerD.image,
        class: "sf-banner--slim sf-banner-right-bottom",
        link: $config.theme.home.bannerD.link,
      },
    ];
    const onSubscribe = (emailAddress) => {
      console.log(`Email ${emailAddress} was added to newsletter.`);
      toggleNewsletterModal();
    };
    const toggleWishlist = (index) => {
      products.value[index].isInWishlist = !products.value[index].isInWishlist;
    };

    return {
      toggleWishlist,
      toggleNewsletterModal,
      onSubscribe,
      banners,
      heroes,
      products,
      menproducts,
      offers,
      typeitems,
    };
  },
};
</script>

<style lang="scss" scoped>
#home {
  box-sizing: border-box;
  padding: 0 var(--spacer-sm);
  @include for-desktop {
    max-width: 2250px;
    padding: 0;
    margin: 0 auto;
  }
/////////////////////////////////////////
}
.men-carousel {
  height: 700px;
  .carousel__item {
    height: 600px;
    &::before {
      content: "";
      width: 22rem;
      height: 380px;
      border: 1px solid var(--_c-braun-primary);
      box-shadow: 0px 0px 2px 2px var(--_c-braun-primary);
      opacity: 0.4;
      margin: -2rem;
      position: absolute;
    }
    .shooping-type-card {
      position: relative;
      cursor: pointer;
      &:hover {
        transition: 0.4s ease-in-out;
        transform: translateY(-25px);
      }
    }
  }
  //Start Change Width for Carousel
  @include for-desktop {
    --carousel-controls-size: 12rem;
  }
  //End Change Width for Carousel
}

.hero {
  --hero-item-background-position: center;
  // start added
  margin: -35px auto var(--spacer-2xl);
  transform: skewY(2deg);
  z-index: 2;
  ul {
    height: 350px;
  }
  // end added
  @include for-mobile {
    margin-top: var(--spacer-2xl);
  }

  .sf-hero-item {
    &:nth-child(even) {
      --hero-item-background-position: left;
      @include for-mobile {
        --hero-item-background-position: 30%;
        ::v-deep .sf-hero-item__subtitle,
        ::v-deep .sf-hero-item__title {
          text-align: right;
          width: 100%;
          padding-left: var(--spacer-sm);
        }
      }
    }
  }
  ::v-deep .sf-hero__control {
    &--right,
    &--left {
      display: none;
    }
  }
}

.banner-grid {
  --banner-container-width: 50%;
  margin: var(--spacer-xl) 0;
  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }
  @include for-desktop {
    margin: var(--spacer-2xl) 0;
    ::v-deep .sf-link {
      --button-width: auto;
      text-decoration: none;
    }
  }
}

.banner {
  &__tshirt {
    background-position: left;
  }
  &-central {
    @include for-desktop {
      --banner-container-flex: 0 0 70%;
    }
  }
}

.similar-products {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: var(--spacer-2xs);
  --heading-padding: 0;
  border-bottom: 1px var(--c-light) solid;
  @include for-desktop {
    border-bottom: 0;
    justify-content: center;
    padding-bottom: 0;
  }
}

.call-to-action {
  background-position: right;
  margin: var(--spacer-xs) 0;
  @include for-desktop {
    //Start Change
    float: left;
    margin-left: 1%;
    width: 32%;
    height: 100%;
    margin: var(--spacer-xl) var(--spacer-xs) var(--spacer-2xl) var(--spacer-xs);
    //End Change
  }
}
.carousel {
  margin: 0 calc(0 - var(--spacer-sm)) 0 0;
  @include for-desktop {
    margin: 0;
  }
  &__item {
    margin: 1.375rem 0 2.5rem 0;
    @include for-desktop {
      margin: var(--spacer-xl) 0 var(--spacer-xl) 0;
    }
    &__product {
      --product-card-add-button-transform: translate3d(0, 30%, 0);
    }
  }
  ::v-deep .sf-arrow--long .sf-arrow--right {
    --arrow-icon-transform: rotate(180deg);
    -webkit-transform-origin: center;
    transform-origin: center;
  }
}
.newsletter-div {
  box-sizing: border-box;
  display: flex;
  flex-direction: row;
}
//start added
.sf-banner-right-top {
  border-top-left-radius: 6%;
}
.sf-banner-right-bottom {
  border-bottom-left-radius: 6%;
}
.sf-banner-left {
  border-top-right-radius: 6%;
  border-bottom-right-radius: 6%;
}
.banner-central {
  transform: skewY(-1deg);
  transition: 0.4s ease-in-out;
  animation: moveskewy 1s infinite alternate;
}
.sf-banner--slim {
  cursor: pointer;
}
@keyframes moveskewy {
  0% {
    transform: skewY(-1deg);
  }
  100% {
    transform: skewY(1deg);
  }
}
.type-store {
  width: 100%;
  max-height: 100rem;
  position: relative;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin-bottom: var(--spacer-3xl);
  margin-top: var(--spacer-3xl);
  @include for-mobile {
    flex-direction: column;
  }
}

//end added
</style>
