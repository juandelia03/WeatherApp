<template>
  <div id="nav">
    <div class="container">
      <div class="searchDiv">
        <input type="text" class="search" v-model="city" @keyup.enter="NewCity" />
      </div>
      <div class="place">
        <h1 class="city">{{ data.name }}</h1>
      </div>
      <div class="ov">
        <h1 class="overall">{{ data.weather[0].main }}</h1>
      </div>
      <div class="Temp" v-if="done === true">
        <h1 class="Temperature">{{ data.main.temp }}</h1>
      </div>
      <div class="min-max">
        <p>{{ data.main.temp_max }}</p>
      </div>
      <div class="min-max">
        <p>{{ data.main.temp_min }}</p>
      </div>
      <div class="wind" v-if="done === true">
        <div class="wrapper">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            class="icon"
          >
            <path
              d="M11 10h-5v-2h5c.552 0 1-.448 1-1s-.448-1-1-1c-.403 0-.747.242-.905.587l-1.749-.956c.499-.965 1.494-1.631 2.654-1.631 3.971 0 3.969 6 0 6zm7 7c0-1.656-1.344-3-3-3h-7v2h7c.552 0 1 .448 1 1s-.448 1-1 1c-.403 0-.747-.242-.905-.587l-1.749.956c.499.965 1.494 1.631 2.654 1.631 1.656 0 3-1.344 3-3zm1.014-7.655c.082-.753.712-1.345 1.486-1.345.827 0 1.5.673 1.5 1.5s-.673 1.5-1.5 1.5h-8.5v2h8.5c1.932 0 3.5-1.568 3.5-3.5s-1.568-3.5-3.5-3.5c-1.624 0-2.977 1.116-3.372 2.617l1.886.728zm-9.014 1.655h-8v2h8v-2zm-4 3h-6v2h6v-2zm-2-6h-4v2h4v-2z"
            />
          </svg>
          <h1 class="indice">{{ data.wind.speed }} m/s</h1>
        </div>
      </div>
      <div class="wind" v-if="done === true">
        <h1 class="indice">{{ data.main.pressure }} hPa</h1>
      </div>
    </div>
  </div>
</template>

<script>
const key = "9ac84832dec1d7c25dc81369d35eb220";
export default {
  data() {
    return {
      city: "United states",
      data: {},
      done: false,
    };
  },
  methods: {
    async getData() {
      this.data = {
        main: {
          temp: "",
          temp_max: "",
          temp_min: "",
        },
        weather: [{ main: "" }],
      };
      let response = fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${key}`
      );
      let data = await (await response).json().then((this.done = true));
      this.data = data;
      this.data.main.temp += "";
      this.data.main.temp_max = "H:" + this.data.main.temp_max + "°";
      this.data.main.temp_min = "L:" + this.data.main.temp_min + "°";
    },
    NewCity() {
      if (this.city != this.data.name) {
        this.getData();
      }
    },
  },
  beforeMount() {
    this.getData();
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");
* {
  padding: 0px;
  margin: 0px;
  font-family: "Roboto";
}
.container {
  min-height: 100vh;
  height: 100%;
  min-width: 100vw;
  background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ9Ui8yuVW5eSTwI12Zpeozwy1bzRgyzLDeiKaQrK1BRjap7bknGD8eWDqN3cU-a13pHUo&usqp=CAU");
  background-repeat: no-repeat;
  background-size: 100% 100%;
}
.searchDiv {
  display: flex;
  justify-content: center;
  padding: 30px;
}
.search {
  height: 50px;
  width: 500px;
  border-radius: 50px;
  border-color: white;
  border-style: none;
  outline: none;
  padding-left: 20px;
  font-weight: 400;
  background-color: rgb(255, 255, 255, 0.5);
  font-size: 18px;
}
.Temp {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.Temperature {
  color: white;
  font-size: 60px;
  font-weight: 300;
}
.Temperature::after {
  position: absolute;
  content: "°";
}
.place {
  display: flex;
  justify-content: center;
  align-content: center;
  margin-top: 60px;
}
.city {
  color: white;
  font-weight: 300;
  font-size: 40px;
}
.ov {
  display: flex;
  justify-content: center;
  margin-top: 12px;
}
.overall {
  color: white;
  font-weight: 200;
  font-size: 24px;
}
.min-max {
  display: flex;
  justify-content: center;
  color: white;
  margin-top: 10px;
  font-weight: 300;
}
.wind {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(255, 255, 255, 0.5);
  width: 100px;
  height: 30px;
  margin: auto;
  margin-top: 25px;
  border-radius: 25px;
}
.indice {
  color: white;
  font-weight: 400;
  font-size: 14px;
}
.icon {
  fill: white;
}
.wrapper {
  display: flex;
  align-items: center;
  gap: 4px;
}
</style>
