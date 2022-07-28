<template>
  <div class="tool-bar">
    <div class="row row-cols-auto">
      <div class="col">
        <button class="type">{{ $t("Button_8") }}</button>
      </div>
      <div class="col">
        <button class="type">{{ $t("Button_8") }}</button>
      </div>
      <div class="col">
        <img src="../../assets/sideplate1.png" alt="logo" />
      </div>
      <div class="col">
        <button class="type">{{ $t("Button_8") }}</button>
      </div>
      <div class="col">
        <button @click="$router.back()" class="type">
          {{ $t("Button_10") }}
        </button>
      </div>
    </div>

    <div class="row row-cols-auto" style="margin: 5px">
      <div class="col1">
        <button class="type">{{ $t("Button_8") }}</button>
      </div>
      <div class="col1">
        <button class="type">{{ $t("Button_8") }}</button>
      </div>
      <div class="col1">
        <button class="type">{{ $t("Button_8") }}</button>
      </div>
      <div class="col1">
        <button class="type" @click="navigateto">
          {{ $t("Button_6") }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    msg: String,
  },
  data() {
    return {
      id: this.$route.params.id,
    };
  },
  methods: {
    navigateto() {
      console.log(this.id);
      this.$router.push({
        name: "ProductsList",
        params: { id: this.$route.params.id },
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tool-bar {
  width: 100%;
  padding: 3% 5%;
}

.row {
  border-radius: 20px;
  background: #2626268c;
    box-shadow: 0 4px 8px 0 rgba(17, 17, 17, 0.529), 0 6px 20px 0 rgba(17, 17, 17, 0.529);
}

img {
  width: 60%;
  border-radius: 30px;
}

button {
  width: 90%;
  padding: 5% 10%; /* Green */
  border: none;
  color: white;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.type {
  border: 1px solid white;
  background: rgba(112, 112, 112, 0.2);
  color: white;
}

.type:hover {
  border: 1px solid rgba(112, 112, 112, 0.2);
  background: rgba(255, 255, 255, 0.486);
  color: white;
}

@media screen and (min-width: 320px) {
}

@media screen and (min-width: 500px) {
}

@media screen and (min-width: 900px) {
  .col {
    width: 20%;
    text-align: center;
    padding: 1% 2%;
  }
  .col1 {
    width: 25%;
    text-align: center;
    padding: 1% 2%;
  }
}
</style>
