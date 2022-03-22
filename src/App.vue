<template>
  <div class="wrapping">
    <div class="about">
      <h1 class="title">Namoz vaqtlari <img :src="islamIco" alt="islam" /></h1>
      <p class="desc">
        Hanafiy mazhabi bo'yicha O'zbekiston shaharlaridagi namoz vaqtlarini
        ko'rsatuvchi ilova
      </p>
    </div>
    <form @submit.prevent="fetchTimes" class="search">
      <input type="text" placeholder="Toshkent" v-model="city" />
      <button type="submit">Jo'natish</button>
      <p class="msg">{{ msg }}</p>
    </form>
    <loader v-if="loading === true" />
    <div class="times" v-else-if="loading === false">
      <div class="meta">
        <p class="city-name">
          <b>{{ city }}</b> namoz vaqtlari
        </p>
        <div class="milodiy-date">
          Milodiy: <i>{{ times.data.date.gregorian.date }}</i>
        </div>
        <div class="hijri-date">
          Hijriy: <i>{{ times.data.date.hijri.date }}</i>
        </div>
        <div class="timezone">
          Zona: <i>{{ times.data.meta.timezone }}</i>
        </div>
      </div>
      <div class="timings">
        <div class="timing-box fajr">
          <h1 class="timing">{{ times.data.timings.Fajr }}</h1>
          <p>Bamdod</p>
        </div>
        <div class="timing-box dhuhr">
          <h1 class="timing">{{ times.data.timings.Dhuhr }}</h1>
          <p>Peshin</p>
        </div>
        <div class="timing-box asr">
          <h1 class="timing">{{ times.data.timings.Asr }}</h1>
          <p>Asr</p>
        </div>
        <div class="timing-box shom">
          <h1 class="timing">{{ times.data.timings.Maghrib }}</h1>
          <p>Shom</p>
        </div>
        <div class="timing-box xufton">
          <h1 class="timing">{{ times.data.timings.Isha }}</h1>
          <p>Xufton</p>
        </div>
      </div>
      <div class="down-meta">
        <p class="sunrise">
          Quyosh chiqishi: <b>{{ times.data.timings.Sunrise }}</b>
        </p>
        <p class="tahajjud">
          Tahajjud: <b>{{ times.data.timings.Midnight }}*</b>
        </p>
        <p class="avvobiyn">Avvobiyn: <b>Shom namozidan keyin*</b></p>
      </div>
    </div>
    <footer>
      <p class="author">
        made by
        <a target="_blank" rel="noreferrer" href="https://dasturchioka.uz"
          >dasturchioka</a
        >
      </p>
    </footer>
  </div>
</template>

<script>
import islamIco from "@/assets/islam.png";

import Loader from "@/components/Loader.vue";

import axios from "axios";

export default {
  components: { Loader },
  name: "App",
  data() {
    return {
      times: {},
      loading: null,
      city: "",
      msg: "",
      islamIco,
    };
  },
  methods: {
    fetchTimes() {
      if (this.city.includes("  ")) {
        this.msg = "Mavjud shahar nomini kiriting!";
      } else if (this.city.length > 1) {
        this.msg = "";
        this.loading = true;
        axios
          .get(
            `https://api.aladhan.com/v1/timingsByCity?city=${this.city}&country=Uzbekistan&method=3&school=1`
          )
          .then((res) => {
            this.times = res.data;
            this.loading = false;
            console.log(res.data.data);
          })
          .catch((err) => {
            this.loading = false;
            console.log(err);
          });
      } else if (this.city.length < 1) {
        this.msg = "Mavjud shahar nomini kiriting!";
        this.loading = null;
      }
    },
  },
};
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Nunito", sans-serif;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding-top: 40px;
}

.wrapping {
  display: flex;
  flex-direction: column;
  padding: 0 20px;
}

footer {
  margin: 20px 0;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.about {
  margin: 20px;
  text-align: center;
}

.about .title {
  font-weight: 800;
}

.search {
  display: flex;
  flex-direction: column;
}

.search input {
  border: 1px solid #ccc;
  border-radius: 100px;
  padding: 10px 15px;
  outline: none;
  transition: all 0.4s ease;
}

.search input:focus {
  border: 1px solid #0848f8;
}

.search button {
  background: #0848f8;
  border: 1px solid #0848f8;
  cursor: pointer;
  outline: none;
  color: #fff;
  border-radius: 100px;
  padding: 10px 15px;
  transition: all 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
  margin-top: 10px;
  font-family: "Nunito", sans-serif;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 700;
}

.search button:hover {
  transform: translateY(-8%);
}

.msg {
  margin-top: 5px;
}

.meta,
.down-meta {
  margin: 15px 0;
}

.meta .city-name {
  font-size: 20px;
}

.city-name b {
  color: #0848f8;
}

.timings {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 10px;
}

.timings .timing-box {
  flex: 1 1 200px;
  background: #0848f8;
  color: #fff;
  padding: 20px;
  text-align: center;
  border-radius: 10px;
}

.author {
  font-size: 20px;
  margin-top: 15px;
}

a {
  text-decoration: none;
  color: #0848f8;
}
</style>
