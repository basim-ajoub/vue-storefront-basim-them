<template>
  <SfTopBar class="topbar">
   <!-- <template #left>
        <SfButton class="sf-button--text">{{ $t('Help & FAQs') }}</SfButton>
      </template>
    <template #center>
        <p>{{ $t('Download') }}</p>
        <SfButton class="topbar__button sf-button--text">{{ $t('Find out more') }}</SfButton>
      </template> -->
      <template #right>
        <StoreLocaleSelector />
    </template>
  </SfTopBar>
</template>

<script>
import { SfButton, SfTopBar } from '@storefront-ui/vue';
import StoreLocaleSelector from './StoreLocaleSelector';
export default {
  components: {
    SfTopBar,
    SfButton,
    StoreLocaleSelector
  }
};

</script>
<style lang="scss" scoped>
.topbar {
  background: var(--_c-dark-secondary);
  position: absolute;
  width: 4%;
border-top-right-radius: 100%;
  //z-index: 2;
  //start added
  height: var(--spacer-md);
  z-index: 4;
    // end added
  &__button {
    margin: 0 0 0 var(--spacer-xs);
  }
}
</style>
