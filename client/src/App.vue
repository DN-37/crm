<template>
  <div>
    <Navbar />

    <div class="is-loading-bar has-text-centered" v-bind:class="{ 'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
      <router-view />
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import Navbar from '@/components/layout/Navbar'
export default {
  name: 'App',
  components: {
    Navbar
  },
  beforeCreate() {
    this.$store.commit('initializeStore')
    if (this.$store.state.token) {
      axios.defaults.headers.common['Authorization'] = "Token " + this.$store.state.token
    } else {
      axios.defaults.headers.common['Authorization'] = ""
    }
    if (!this.$store.state.team.id) {
      this.$router.push('/dashboard/add-team')
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma';

.lds-dual-ring {
  display: inline-block;
  width: 8rem;
  height: 8rem;
}

.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 6.4rem;
  height: 6.4rem;
  margin: 0.8rem;
  border-radius: 50%;
  border: 0.6rem solid #ccc;
  border-color: #ccc transparent #ccc transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}

@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;

  &.is-loading {
    height: 8rem;
  }
}
</style>
