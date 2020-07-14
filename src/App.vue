<template>
  <div id="app" class="ui container segment">
    <h1>GitHub Profiles</h1>

    <div class="ui fluid large input" icon="input" style="margin-bottom: 5px;">
      <input type="text" placeholder="user..." v-model="username" v-on:keyup.enter="loadNewUser()" v-on:input="destroy()" style="width: fit-content"/>
    </div>


    <div v-if="userFound()">
      <div class="ui centered card">
        <div class="image">
          <img :src="user.avatar_url">
        </div>
        <div class="content">
          <a class="header">{{user.name}}</a>
          <div class="meta">
            <span class="date">Joined in {{user.created_at}}</span>
          </div>
          <div class="description">
            {{user.location}}
          </div>
        </div>
        <div class="extra content">
          <a>
            <i class="user icon"></i>
            {{user.followers}}
          </a>
        </div>
      </div>
    </div>
    <div v-else>
      <div class="ui icon message">
        <i class="notched circle loading icon"></i>
        <div class="content">
          <div class="header">
            Aguardando busca...
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      username: "",
      user: {}
    }
  },
  methods: {
    userFound: function() {
          return !(Object.keys(this.user).length === 0);
        },
        loadNewUser: function() {
          let options = {
            method: 'GET',
            "content-type": 'application/json'
          };

          let response = fetch(`https://api.github.com/users/${this.username}`, options);

          response.then((response) => {
            return response.json();
          }).then((json) => {
            this.user = json;
            this.user.created_at = new Date(this.user.created_at).getFullYear();
          }).catch((error) => {
            console.error(error);
          });
        },
        destroy: function() {
          this.user = {};
        }
  }
}
</script>

<style>
  @import url('https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.4.1/semantic.min.css');
</style>
