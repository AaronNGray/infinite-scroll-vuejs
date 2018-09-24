<template>
  <div id="app">
    <h1>Random User</h1>
    <div class="person" v-for="(person, index) in persons" :key="index">
      <div class="left">
        <img :src="person.picture.large" alt="">
      </div>
      <div class="right">
        <p>{{ person.name.first}} {{ person.name.last }}</p>
          <div class="text-capitalize">
            <strong>Location:</strong> {{ person.location.city}}, {{ person.location.state }}
          </div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
export default {
  name: 'app',
  data() {
    return {
      persons: []
    }
  },
  methods: {
    getInitialUsers() {
      axios.get(`https://randomuser.me/api/?results=5`).then(response => {
        this.persons = this.persons.concat(response.data.results)
      })
    },
    scroll(person) {
      let isLoading = false
      window.onscroll = () => {
        let bottomOfWindow = document.documentElement.offsetHeight - this.getScrollTop() - window.innerHeight <= ((window.innerHeight / 3) * 2)
        if (bottomOfWindow && isLoading == false) {
          isLoading = true
          axios.get(`https://randomuser.me/api/?results=5`).then(response => {
            this.persons = this.persons.concat(response.data.results)
            console.log(response.data);
            isLoading = false
          })
        }
      }
    },
    getScrollTop() {
      return window.pageYOffset || documentElement.scrollTop || body.scrollTop || 0;
    }
  },
  beforeMount() {
    this.getInitialUsers()
  },
  mounted() {
    this.scroll(this.persons)
  }
}
</script>

<style lang="scss">
.person {
  background: #ccc;
  border-radius: 2px;
  width: 20%;
  margin: 0 auto 15px auto;
  padding: 15px;

  img {
    width: 100%;
    height: auto;
    border-radius: 2px;
  }

  p:first-child {
    text-transform: capitalize;
    font-size: 2rem;
    font-weight: 900;
  }

  .text-capitalize {
    text-transform: capitalize;
  }
}
</style>
