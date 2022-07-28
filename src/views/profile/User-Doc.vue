<template>
  <div class="user-doc">
    <div class="doc-box">

      <div v-if="working">

        <div class="input-group mb-3" v-if="user.verified ===false ">
          <input type="file" class="form-control" aria-describedby="inputGroupFileAddon04" @change="handleFileUpload1" />
          <button class="btn btn-outline-secondary" type="button" @click="sendcard">
            ارسال کارت بانکی
          </button>
        </div>

        <div class="input-group mb-3" v-if="user.verified ===false ">
          <progress max="100" :value.prop="uploadPercentage1"></progress>
        </div>

        <div class="input-group mb-3" v-if="user.verified ===false ">
          <input type="file"  class="form-control" aria-describedby="inputGroupFileAddon04" @change="handleFileUpload2" />
          <button class="btn btn-outline-secondary" type="button" @click="sendidentity">
            ارسال احراز هویت
          </button>
        </div>

        <div class="input-group mb-3" v-if="user.verified ===false ">
          <progress max="100" :value.prop="uploadPercentage2"></progress>
        </div>

        <div v-if="user.verified ===false ">
          <input type="text" placeholder="استان" v-model="state" />
          <input type="text"  placeholder="شهر" v-model="city" />
        </div>


        <div v-if="user.verified ===false ">
          <input type="text" placeholder="شماره شبا" v-model="sheba" />
          <input type="text" placeholder="کد پستی" v-model="postalcode" />
        </div>

        <div v-if="user.verified ===false ">
          <textarea v-model="address" placeholder="آدرس"></textarea>
        </div>

        <br>

        <center v-if="user.verified === true ">
          <h3>
            احراز هویت شما کامل شده است
          </h3>

        <br>

        <div>
          <div class="row row-cols-2">
            <div class="col">
            نام : 
              {{ user.firstname }}
            </div>
            <div class="col">
            نام خانوادگی : 
              {{ user.lastname }}
            </div>
            <div class="col">
            ایمیل : 
              {{ user.email }}
            </div>

            <div class="col">
            تلفن : 
              {{ user.phone }}
            </div>
            <div class="col">
            شهر : 
              {{ user.city }}
            </div>
            <div class="col">
            استان : 
              {{ user.province }}
            </div>
            
            <div class="col">
            کدپستی : 
              {{ user.postalcode }}
            </div>
            <div class="col">
            شبا : 
              {{ user.sheba }}
            </div>
            <div class="col">
            کارت : 
              {{ user.card }}
            </div>
          </div>
        </div>
        </center>

        <center>
          <button id="update" @click="submit"  v-if="user.verified ===false ">ارسال</button>
        </center>
      </div>
      <div v-else class="doc-form">
        <center>
          <h3>
            {{ $t("Button_8") }}
          </h3>
        </center>
      </div>
    </div>

    <!-- <div class="doc-box">
      <h5>کتاب‌های شما</h5>

      <div class="books-list">
        <div class="row row-cols-auto">
          <div class="col">
            <div class="add-book">
              <center>
                {{ $t("Button_12") }}
              </center>
            </div>
          </div>
        </div>
      </div>
    </div> -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    id: String,
  },
  data() {
    return {
      userid: localStorage.getItem("userid") ,
      working: true,

      city:"",
      state:"",
      address:"",
      postalcode:"",
      sheba:"",
      cimg:null,
      iimg:null,

      url: process.env.VUE_APP_API_URL,
      uploadPercentage1: 0,
      uploadPercentage2: 0,
      user:{},
    };
  },
  methods:{
    handleFileUpload1(event) {
      this.cimg = event.target.files[0];
    },
    handleFileUpload2(event) {
      this.iimg = event.target.files[0];
    },
    submit(){
      // let formData = new FormData();

      const formy = {
          city: this.city,
          province: this.state,
          address: this.address,
          sheba: this.sheba,
        };

      console.log(
        formy
      );

      axios.put(
        process.env.VUE_APP_API_URL + "users/ident/" + localStorage.getItem("userid"),
        formy,
        {
          onUploadProgress: function (progressEvent) {
            this.uploadPercentage = parseInt(
              Math.round((progressEvent.loaded / progressEvent.total) * 100)
            );
          }.bind(this),
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
      .then(function (res) {
        if (res.status == 200) {
          console.log("Done");
        }else{
          console.log("Error");
        }
      })

    },
    sendcard(){
      let formdata = new FormData();
      formdata.append("image", this.cimg);
      let url = process.env.VUE_APP_API_URL + "users/images/pics/" + localStorage.getItem("userid")
      axios.post(url, formdata, {
          onUploadProgress: function (progressEvent) {
            this.uploadPercentage1 = parseInt(
              Math.round((progressEvent.loaded / progressEvent.total) * 100)
            );
          }.bind(this),
          headers: {
            "Content-Type": "multipart/form-data",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        },
      )
      .then(function (res) {
        if(res.status === 200){
          window.location.reload()
          alert("Done")
        }
        else{
          alert(Error)
        }
      })
    },
    sendidentity(){
      let formdata = new FormData();
      formdata.append("image", this.iimg);
      let url = process.env.VUE_APP_API_URL + "users/images/pics/" + localStorage.getItem("userid")
      axios.post(url, formdata, {
          onUploadProgress: function (progressEvent) {
            this.uploadPercentage2 = parseInt(
              Math.round((progressEvent.loaded / progressEvent.total) * 100)
            );
          }.bind(this),
          headers: {
            "Content-Type": "multipart/form-data",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        },
      )
      .then(function (res) {
        if(res.status === 200){
          window.location.reload()
          alert("Done")
        }
        else{
          alert(Error)
        }
      })
    }
  },
  mounted() {

    let newurl = process.env.VUE_APP_API_URL + "users/" + this.userid;

    axios
      .get(newurl, {
        headers: { Authorization: "Bearer " + localStorage.getItem("token") },
      })
      .then((response) => {
        if (response.status == 200) {
          this.user = response.data;
        } else {
          alert("خطا : " + response.status + " , " + " مشکل در دریافت اطلاعات کاربر ")
        }
      });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user-doc {
  width: 100%;
  overflow: hidden;
  align-items: center;
}

.doc-form {
  padding: 5%;
}

.doc-box {
  border-radius: 20px;
  color: white;
  background: #2626268c;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  padding: 3%;
  margin: 10px 20px 30px 20px;
}

#update {
  width: 100px;
  padding: 10px 20px; /* Green */
  border: 1px solid white;
  background: rgba(112, 112, 112, 0.2);
  color: white;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

#update:hover {
  border: 1px solid rgba(112, 112, 112, 0.2);
  background: rgba(255, 255, 255, 0.486);
  color: white;
}

input {
  width: 50%;
  padding: 1%;
}

textarea{
  width: 100%;
  padding: 2%;
}

input,
textarea,
span,
button {
  border-radius: 5px;
  background: #fff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.books-list {
  padding: 3%;
}

.add-book {
  width: 96%;
  margin: 2%;
  padding: 10%;
  border: 1px solid white;
  border-radius: 15px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

table, tr{
  width: 100%;
}

td {
  width: 50%;
}

@media screen and (min-width: 320px) {
}

@media screen and (min-width: 500px) {
}

@media screen and (min-width: 900px) {
  .col {
    width: 33%;
    padding: 2%;
  }
}
</style>
