<template>
  <div class="user-info">
    <div class="info-box">
      <h5>
        {{ $t("title_1") }}
      </h5>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="40%"
        height="40%"
        fill="white"
        class="bi bi-person-circle"
        viewBox="0 0 16 16"
      >
        <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0z" />
        <path
          fill-rule="evenodd"
          d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8zm8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1z"
        />
      </svg>
      <h6>{{ user.firstname }} {{ user.lastname }}</h6>
      <p>
        {{ user.phone }}
      </p>
      <!-- <button> {{ $t("Button_11") }} </button> -->
    </div>
    <div class="info-box">
      <h5>
        {{ $t("title_3") }}
      </h5>

      <p>No Accounts Yet</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    info: String,
  },
  data() {
    return {
      user: {},
    };
  },
  mounted() {
    let newurl = process.env.VUE_APP_API_URL + "users/" + localStorage.getItem("userid");

    axios
      .get(newurl, {
        headers: { Authorization: "Bearer " + localStorage.getItem("token") },
      })
      .then((response) => {
        if (response.status == 200) {
          this.user = response.data;
        }else {
          alert("خطا : " + response.status + " , " + " مشکل در دریافت اطلاعات کاربر ")
        }
      });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user-info {
  width: 100%;
  overflow: hidden;
}

.info-box {
  border-radius: 20px;
  color: white;
  background: #2626268c;
    box-shadow: 0 4px 8px 0 rgba(17, 17, 17, 0.529), 0 6px 20px 0 rgba(17, 17, 17, 0.529);
  padding: 10px;
  margin: 10px 20px 30px 20px;
  text-align: center;
}

svg {
  margin: 5%;
}

button {
  width: 90%;
  padding: 5% 10%; /* Green */
  border: 1px solid white;
  background: #80B918;
  color: white;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 10px 2px;
  cursor: pointer;
  border-radius: 20px;
}

button:hover {
  border: 1px solid #80B918;
  background: white;
  color: #80B918;
}

@media screen and (min-width: 320px) {
}

@media screen and (min-width: 500px) {
}

@media screen and (min-width: 900px) {
}
</style>
