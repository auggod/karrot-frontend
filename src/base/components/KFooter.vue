<template>
  <div class="footer row">
    <div class="k-logo-container col-xl-1 col-xs-4">
      <div>
        <RouterLink :to="'/'">
          <KarrotLogo/>
        </RouterLink>
      </div>
    </div>
    <small class="row group col-xl-4 col-xs-8">
      <span>
        karrot
        <a
          v-if="release"
          :href="release.link"
          target="_blank"
          rel="noopener"
        >
          {{ release.name }}
        </a>
        &mdash; made with
        <i class="fas fa-heart" />
        by
        <a
          href="https://foodsaving.world"
          target="_blank"
          rel="noopener"
        >
          foodsaving worldwide
        </a>
      </span>
    </small>
    <small class="row justify-center group col-xl-7 col-xs-12">
      <a
        href="https://github.com/yunity/karrot-frontend"
        target="_blank"
        rel="noopener"
      >
        <i class="fab fa-fw fa-github" />
        {{ $t('GLOBAL.GITHUB_NOTE') }}
      </a>
      <a
        href="https://foodsaving.world"
        target="_blank"
        rel="noopener"
      >
        <i class="fas fa-fw fa-globe" />
        {{ $t('GLOBAL.FSWW_NOTE') }}
      </a>
      <a
        href="https://community.foodsaving.world/"
        target="_blank"
        rel="noopener"
      >
        <i class="fab fa-fw fa-discourse" />
        {{ $t('GLOBAL.COMMUNITY_NOTE') }}
      </a>
      <a href="mail:karrot@foodsaving.world">
        <i class="fas fa-fw fa-envelope"/>
        karrot@foodsaving.world
      </a>
    </small>
  </div>
</template>

<script>
import KarrotLogo from '@/logo/components/KarrotLogo'
import { mapGetters } from 'vuex'

export default {
  components: { KarrotLogo },
  computed: {
    ...mapGetters({
      deployed: 'about/deployed',
    }),
    release () {
      if (__ENV.DEV) {
        return {
          link: '',
          name: 'local dev',
        }
      }
      if (this.deployed) {
        if (this.deployed.env === 'production') {
          return {
            link: 'https://github.com/yunity/karrot-frontend/blob/master/CHANGELOG.md',
            name: this.deployed.date,
          }
        }
        if (this.deployed.env === 'development') {
          return {
            link: `https://github.com/yunity/karrot-frontend/tree/${this.deployed.commitSHA}`,
            name: this.deployed.date,
          }
        }
      }
      else {
        return {
          link: '',
          name: '',
        }
      }
    },
  },
}
</script>

<style scoped lang="stylus">
@import '~variables'
.footer
  padding-top 10px
  padding-bottom 10px
  a
    color $secondary
    i
      color black
  background-color rgba(255, 255, 255, 0.8)
  width 100%
.k-logo-container
  position relative
  height 55px
  > div
    position absolute
    top -20px
    left 20px
    height 80px
    filter: drop-shadow(0px 1px 1px rgba(0,0,0,.7) )
</style>
