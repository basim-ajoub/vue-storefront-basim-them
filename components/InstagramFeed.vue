<template>
  <SfSection
    title-heading= "Share Your Look"
    subtitle-heading="#YOURLOOK"
    class="title"
  >
    <div class="grid grid-images">
      <div class="grid__row">
        <div class="grid__col">
          <SfImage
            class="img-comp-left"
            v-if="isMobile"
            src="/homepage/imageAm.webp"
            alt="katherina_trn"
            :width="160"
            :height="160"
            >katherina_trn</SfImage
          >
          <SfImage
            class="img-comp-left"
            v-else
            src="/homepage/imageAd.webp"
            alt="katherina_trn"
            :width="470"
            :height="470"
            >katherina_trn</SfImage
          >
        </div>
        <div class="grid__col small">
          <SfImage
            class="img-comp-left"
            v-if="isMobile"
            src="/homepage/imageBm.webp"
            alt="katherina_trn"
            :width="160"
            :height="160"
            >katherina_trn</SfImage
          >
          <SfImage
            class="img-comp-left"
            v-else
            src="/homepage/imageCd.webp"
            alt="katherina_trn"
            :width="470"
            :height="160"
            >katherina_trn</SfImage
          >
        </div>
      </div>
      <div class="grid__row">
        <div class="grid__col small">
          <SfImage
            class="img-comp-right"
            v-if="isMobile"
            src="/homepage/imageCm.webp"
            alt="katherina_trn"
            :width="160"
            :height="160"
            >katherina_trn</SfImage
          >
          <SfImage
            class="img-comp-right"
            v-else
            src="/homepage/imageBd.webp"
            alt="katherina_trn"
            :width="470"
            :height="160"
            >katherina_trn</SfImage
          >
        </div>
        <div class="grid__col">
          <SfImage
            class="img-comp-right"
            v-if="isMobile"
            src="/homepage/imageDm.webp"
            alt="katherina_trn"
            :width="160"
            :height="160"
            >katherina_trn</SfImage
          >
          <SfImage
            class="img-comp-right"
            v-else
            src="/homepage/imageDd.webp"
            alt="katherina_trn"
            :width="470"
            :height="470"
            >katherina_trn</SfImage
          >
        </div>
      </div>
    </div>
  </SfSection>
</template>
<script>
import { SfSection, SfImage } from "@storefront-ui/vue";
import {
  mapMobileObserver,
  unMapMobileObserver,
} from "@storefront-ui/vue/src/utilities/mobile-observer.js";
export default {
  name: "InstagramFeed",
  components: {
    SfSection,
    SfImage,
  },
  computed: {
    ...mapMobileObserver(),
  },
  beforeDestroy() {
    unMapMobileObserver();
  },
};
</script>
<style lang="scss" scoped>
.title {
  --heading-title-font-weight: var(--font-weight--semibold);
  --section-margin: var(--spacer-xl) 0;
  --section-content-margin: var(--spacer-xl) 0;
  --heading-title-font-size: var(--h2-font-size);
  @include for-desktop {
    --section-margin: var(--spacer-2xl) 0;
    --section-content-margin: var(--spacer-2xl) 0;
  }
}
.grid {
  display: flex;
  max-height: 20.625rem;

  width: 100%;
  justify-content: center;
  margin: 0;
  //Start Change in Image Overlay
  --image-overlay-background: 0.1; //change overlay opacity
  --image-overlay-color: var(--_c-dark-primary); //Change font of OVERLAY
  //ENd Change in Image Overlay

  @include for-desktop {
   // max-height: 40.625rem;
    max-width: 60rem;
    margin: 0 auto;
    //Start Change
      max-height: 44.625rem;
    

    .img-comp-left {
      transform-origin: center;
      transform: perspective(800px) rotateY(25deg);
      transition: 0.5s;
      cursor: pointer;
      margin-top: var(--spacer-sm);

    }
    .img-comp-right {

      transform-origin: center;
      transform: perspective(800px) rotateY(-25deg);
      transition: 0.5s;
      cursor: pointer;
    }
    &:hover .img-comp-left,
    .img-comp-right {
      opacity: 0.3;
    }
    .img-comp-left:hover,
    .img-comp-right:hover {
      opacity: 1;
      transform: rotateY(0deg);
    }
    //End Change
  }
  &__row {
    display: flex;
    flex-direction: column;
    & + & {
      margin-left: var(--spacer-xs);
      @include for-desktop {
        margin-left: var(--spacer-sm);
      }
    }
  }
  &__col {
    width: 10rem;
    height: 10rem;
    @include for-desktop {
      &.small {
        height: 10rem;

        //Start Change Small Image

        margin-bottom:var(--spacer-base);
        //End Change Small Image
        }
      width: 29.375rem;
      height: 29.375rem;
    }
    & + & {
      margin-top: var(--spacer-xs);
      @include for-desktop {
        margin-top: var(--spacer-xl);

      }
    }
  }
}
</style>
