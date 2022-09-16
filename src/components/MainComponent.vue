<template>
  <div class="pt140">
    <h1>Working with GET request</h1>
    <div class="cards" v-if="users.length > 0">
      <div class="card"  v-for="(user, index) in users" :key="index">
        <img :src="user.photo" @error.once="imageUrlAlt" :alt="user.name">
        <p class="m20">{{ user.name}}</p>
        <p class="m0">{{ user.position}}</p>
        <p class="m0"><a class="mail_link" :href="`mail:${user.email}`" :title="user.email">{{ user.email}}</a></p>
        <p class="m0"><a class="mail_link" :href="`tel:${user.phone}`" :title="user.phone">{{ user.phone}}</a></p>
      </div>
    </div>
    <div v-else>
      <div class="loader">
      </div>
    </div>
    <div class="mt50">
      <button class="y_btn" @click="more">Show more</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainComponent',
  beforeMount(){
    this.getUsers();
  },

  data() {
    return {
      users :[],
      page : 1,
      total_pages: 1,
      count: 6,
    }
  },
  methods: {
    async getUsers(){

       await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=${this.page}&count=${this.count}`)
           .then(res => { return res.json();})
           .then( data => {
             if (data.success) {
               this.total_pages = data.total_pages;
               this.users = data.users.sort((a, b) => Date.parse(b.registration_timestamp) - Date.parse(a.registration_timestamp))
             }
           }
       ).catch(e => { console.log(e)})
    },

    imageUrlAlt(event) {
      event.target.src = require('@/assets/photo-cover.svg');
    },

    more() {
      if(this.page < this.total_pages)
        this.page +=1;
      else
        this.page= 1;

      this.getUsers();
    }
  }
}
</script>


<style lang="scss" scoped>
.cards{
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  margin: 10px;
  .card{
    width: 230px;
    padding: 20px;
    margin: 20px;
    background-color: #fff;
    border-radius: 10px;
    img {
      border-radius: 50%;
    }
    .mail_link{
      text-decoration: none;
      color: #000;
    }
    p{
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }
}

</style>
