<template>
  <div class="profile">
    <ToolBar></ToolBar>

    <div class="row row-cols-auto">
      <div class="col col1">
        <UserInfo ></UserInfo>
      </div>
      <div class="col col2">
        <UserDoc ></UserDoc>
      </div>
    </div>
  </div>
</template>

<script>
import ToolBar from "../views/profile/Profile-Toolbar.vue";
import UserInfo from "../views/profile/User-Info.vue";
import UserDoc from "../views/profile/User-Doc.vue";
import axios from "axios";

export default {
  props: {
    person: Object,
  },
  components: {
    ToolBar,
    UserInfo,
    UserDoc,
  },
  data() {
    return {
      personinfo: null,
      url: localStorage.getItem("userid"),
    };
  },
  mounted() {
    console.log(localStorage.getItem("userid"));
    // this.url = this.$route.params.id;
    let newurl = process.env.VUE_APP_API_URL + "users/" + this.url;

    axios
      .get(newurl, {
        headers: { Authorization: "Bearer " + localStorage.getItem("token") },
      })
      .then((response) => {
        if (response.status == 200) {
          this.personinfo = response.data;
        }
      });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.profile {
    width: 100%;
    background: url("../assets/profile-wp.jpg") ;
    background-repeat: no-repeat;
    background-size: cover ;
    background-attachment: fixed;
    background-position: center;
    overflow: hidden;
    min-height: 100vh;
  }

.row {
  width: 100%;
}

@media screen and (min-width: 320px) {
}

@media screen and (min-width: 500px) {
}

@media screen and (min-width: 900px) {

  .col1 {
    width: 25%;
  }
  .col2 {
    width: 75%;
  }
}
</style>
