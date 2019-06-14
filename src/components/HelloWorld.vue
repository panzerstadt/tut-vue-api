<template>
  <div class="hello">
    <h1>Your IP is {{ip}}</h1>
    <input type="text" v-model="input.firstname" placeholder="First Name">
    <input type="text" v-model="input.lastname" placeholder="Last Name">
    <button v-on:click="sendData()">Send</button>
    <br>
    <br>
    <textarea name="response" id="response" cols="30" rows="10" v-model="response"></textarea>
    <br>
    <a
      target="_blank"
      rel="noopener noreferer"
      href="https://www.thepolyglotdeveloper.com/2017/10/consume-api-data-http-vuejs-web-application/"
    >tutorial from here</a>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      ip: "",
      input: {
        firstname: "",
        lastname: ""
      },
      response: ""
    };
  },
  mounted() {
    // compoinentDidMount
    axios({ method: "GET", url: "https://httpbin.org/ip" }).then(
      result => {
        this.ip = result.data.origin.split(",")[0];
      },
      error => {
        this.response = JSON.parse(error);
      }
    );
  },
  methods: {
    sendData() {
      axios({
        method: "POST",
        url: "https://httpbin.org/post",
        data: this.input,
        headers: { "content-type": "application/json" }
      }).then(
        result => {
          this.response = JSON.stringify(
            JSON.parse(result.data.data),
            false,
            2
          );
        },
        error => {
          this.response = JSON.parse(error);
        }
      );
    }
  }
};
</script>

<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: "42b983";
}

textarea {
  width: 600px;
  height: 200px;
}
</style>
