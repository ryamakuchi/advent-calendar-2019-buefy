<template>
  <div>
    <b-navbar class="has-shadow is-spaced">
      <template slot="brand">
        <logo />

        <a
          href="https://qiita.com/advent-calendar/2019/nuxt-js"
          target="_blank"
          rel="noopener noreferrer"
          class="level-item has-text-weight-bold has-text-primary"
        >
          Nuxt.js Advent Calendar 2019 for Qiita
        </a>
      </template>

      <template slot="end">
        <b-navbar-item tag="div">
          <a
            href="https://buefy.org/"
            target="_blank"
            rel="noopener noreferrer"
            class="button is-primary"
          >
              Made with Buefy
          </a>
        </b-navbar-item>
      </template>
    </b-navbar>

    <nuxt />
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
export default {
  components: {
    Logo
  }
}
</script>
