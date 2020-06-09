<template>
  <div class="container p-5 text-center">
    <h1>Search user from github</h1>
    <form v-on:submit="auth" method="post">
      <div class="input-group">
        <input class="form-control" v-model="user" type="text" placeholder="Usuário github" />
        <div class="input-group-append">
          <button class="btn border btn-primary" type="submit">
            <span class="fa fa-search"></span>
          </button>
        </div>
      </div>
    </form>
    <div class="row mt-2 p-2">
      <div class="col-md">
        <img
          clas="img-thumbnail"
          style="width: 250px; border-radius: 125px;"
          v-if="avatar_url !== false"
          :src="avatar_url"
        />
        <br />
        <br />
        <span v-if="login !== ''" class="text-muted">
          <strong>login:</strong>
          {{login}}
        </span>
        <br />
        <span v-if="bio !== ''" class="text-muted">
          <strong>bio:</strong>
          {{bio}}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Form",
  data: function() {
    return {
      user: "",
      avatar_url: false,
      login: "",
      bio: ""
    };
  },
  methods: {
    auth: async function(event) {
      try {
        event.preventDefault();

        if (!this.user.trim()) {
          return console.log("Informe usuáiro!");
        }

        const response = await axios.get(
          `https://api.github.com/users/${this.user}`
        );

        const user = response.data;

        this.avatar_url = user.avatar_url;
        this.login = user.login;
        this.bio = user.bio;
      } catch (error) {
        this.avatar_url = false;
        this.login = "";
        this.bio = "";
      }
    }
  }
};
</script>