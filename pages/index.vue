<template>
  <div class="topPage" :class="{ sp: $device.isMobileOrTablet }">
    <Header />
    <div class="artsContainer">
      <ArtItem v-for="(item, i) in arts" :key="i" :art="item" />
    </div>
    <ArtSubmissionButton />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import { Art } from '~/types/entity'
import ArtItem from '~/components/ArtItem.vue'
import Header from '~/components/Header.vue'
import ArtSubmissionButton from '~/components/ArtSubmissionButton.vue'
@Component({
  components: { ArtSubmissionButton, Header, ArtItem },
})
export default class TopPage extends Vue {
  async fetch({ store }) {
    if (store.state.art.items.length === 0) {
      await store.dispatch('art/fetch')
    }
  }

  private get arts(): Art[] {
    return this.$store.state.art.items
  }
}
</script>

<style scoped lang="stylus">
@require '~@/assets/style/variables'
@require '~@/assets/style/mixin'

.topPage
  .artsContainer
    max-width $contents_width
    display grid
    grid-template-columns repeat(auto-fill, minmax(200px, 1fr))
    grid-column-gap $margin_8
    grid-row-gap $margin_8
    box-sizing border-box
    margin $margin_48 auto 0

  .artSubmissionButton
    margin-top $margin_80

  &.sp
    .artsContainer
      grid-row-gap $margin_24
      background-color $white_fff
      margin-top $margin_24
</style>
