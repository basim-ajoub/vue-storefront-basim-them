<template>
  <SfFooter :column="4" multiple class="footer">
    <SfFooterColumn class="footer-cont" :title="$t('About us')">
      <SfList>
        <SfListItem v-for="item in aboutUs" :key="item">
          <SfMenuItem :label="$t(item)" />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn class="footer-cont" :title="$t('Departments')">
      <SfList>
        <SfListItem v-for="item in departments" :key="item">
          <SfMenuItem :label="$t(item)" />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn class="footer-cont" :title="$t('Help')">
      <SfList>
        <SfListItem v-for="item in help" :key="item">
          <SfMenuItem :label="$t(item)" />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn class="footer-cont" :title="$t('Payment & Delivery')">
      <SfList>
        <SfListItem v-for="item in paymentsDelivery" :key="item">
          <SfMenuItem :label="$t(item)" />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
 <!--<SfFooterColumn class="footer-cont " title="Social">-->
      <div class="footer__socials footer-social">
        <SfImage
          class="footer__social-image"
          v-for="item in social"
          :key="item"
          :src="'/icons/' + item + '.svg'"
          :alt="item"
          width="32"
          height="32"
        />
      </div>
   <!--</SfFooterColumn>-->
  </SfFooter>
</template>

<script>
import { SfFooter, SfList, SfImage, SfMenuItem } from "@storefront-ui/vue";

export default {
  components: {
    SfFooter,
    SfList,
    SfImage,
    SfMenuItem,
  },
  data() {
    return {
      aboutUs: ["Who we are", "Quality in the details", "Customer Reviews"],
      departments: ["Women fashion", "Men fashion", "Kidswear", "Home"],
      help: ["Customer service", "Size guide", "Contact us"],
      paymentsDelivery: ["Purchase terms", "Guarantee"],
      social: ["facebook", "pinterest", "google", "twitter", "youtube"],
      isMobile: false,
      desktopMin: 1024,
    };
  },
};
</script>

<style lang="scss">
.footer {
  margin-bottom: 3.75rem;
  //Start Change Footer Background
 background:var(--_c-dark-primary) !important;

  @include for-desktop {
    margin-bottom: 0;
    margin-top:var(--spacer-3xl) !important;
   //End Change Footer Background
  }
  &__socials {
    display: flex;
    justify-content: space-between;
    margin: 0 auto var(--spacer-lg);
    padding: var(--spacer-base) var(--spacer-xl);
    @include for-desktop {
      justify-content: flex-start;
      padding: var(--spacer-xs) 0;
      margin: 0 auto;
    }
  }
  &__social-image {
    margin: 0 var(--spacer-2xs) 0 0;
  }
}

.sf-footer {
  background: (--_c-white-paige-primary);
  //Start Change Footer
  transform: skewY(-2.5deg);
  padding: var(--spacer-base) 5%;
  border-top: var(--spacer-xl) solid var(--_c-paige-primary);
  --footer-column-title-font-size: var(--h6-font-size);
  .sf-footer-column {
    //Start Change the Header Of The List
    &__title {
      width: 80%;
      --footer-column-title-font-size: var(--h4-font-size);
color: var(--_c-paige-primary);
 border-bottom: 1px solid  var(--_c-paige-primary);
    }




    //End Change the Header Of The List
  } .footer-social {
      width: 100%;
      height:var(--spacer-xl);
   display: flex;
   box-sizing: border-box;
   flex-direction: row;
   justify-content: space-around;
   align-content: center;
//background: rgba(255, 0, 0, 0.342);
.footer__social-image{
//  margin: -1.3rem var(--spacer-2xs) 0 0;
  @include for-mobile{
    margin: -1.3rem var(--spacer-2xs) 0 0;
  }
   @include for-desktop{
    margin: -0.3rem var(--spacer-2xs) 0 0;
  }
}
//background: red;
      position: absolute;
  //    top: -45px;
 top:calc(var(--spacer-xl) - var(--spacer-2xl) );
      left: 0;
  //    background: #eabf9f00;
      transform: skewY(0deg);
    }
  @include for-desktop {
    transform: skewY(-2.5deg);
    padding: var(--spacer-base) 5%;
    border-top: var(--spacer-xl) solid var(--_c-paige-primary);

    .footer-cont {
      transform: skewY(2.5deg);
    }

    //End Change Footer
    //  border-top: var(--spacer-xs) solid var(--_c-white-paige-primary);
    padding-bottom: 0;
    margin-top: var(--spacer-2xl);
  }
  &__container {
    margin: var(--spacer-sm);
    @include for-desktop {
      max-width: 69rem;
      margin: 0 auto;
    }
  }
}
</style>
