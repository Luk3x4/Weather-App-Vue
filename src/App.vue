<template>
  <div class="container">
    <div class="cont">
      <div class="inputs">
        <input type="text" placeholder="შეიყვანეთ ქალაქი" v-model="query" @keypress.enter="getData(query)">
        <button @click="getData(query)"> ძიება </button>
      </div>
      <!-- <center> -->
      <p> {{getDate}} </p> 
      <div class="to-center">
      <div class="weather">
        <div class="weather-el">
          <h3 v-if="!notFound" > {{ location }}</h3>
          <h1 ref="test" v-if="!notFound" :class="degrees == 'შეიყვანეთ ქალაქი'?'empty':''">  {{ degrees }}</h1>
          <h2 class="notFound" v-if="notFound">ქალაქი არ მოიძებნა </h2>
          <div class="position" v-if="!notFound">
            <h4>{{ long }}</h4>
            <h4> {{ lat }} </h4>
          </div>
          <h3 class="wind" v-if="!notFound" v-html="windSpeed"></h3>
        </div>
      </div>
      </div>
      <!-- </center> -->
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      query: '',
      location: '',
      degrees: 'შეიყვანეთ ქალაქი',
      long: '',
      lat: '',
      windSpeed: '',
      notFound: false,
      days: ['კვირა', 'ორშაბათი', 'სამშაბათი', 'ოთხშაბათი', 'ხუთშაბათი', 'პარასკევი', 'შაბათი'],
      months: ['იანვარი', 'თებერვალი', 'მარტი', 'აპრილი', 'მაისი', 'ივნისი', 'ივლისი', 'აგვისტო', 'სექტემბერი', 'ოქტომბერი', 'ნოემბერი', 'დეკემბერი'],
      dayNum: new Date().getDay(),
      date: new Date().getDate(),
      month: new Date().getMonth(),
      windowWidth: window.innerWidth,
    }
  },

  computed: {
    getDate() {
      return `${this.days[this.dayNum]}, ${this.date} ${this.months[this.month]}`
    },
    getScreenSize() {
      return this.windowWidth
    }
  },

  methods: {
    async getData(city) {
      const data = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=98c97de810cb707cd1554021d107e09c`).then(data => data.json());
      if(data.cod == '404' || this.query == ''){
        this.notFound = true
      }else{
        console.log(data)
        this.notFound = false
        this.location = data.name
        this.long = `Long: ${data.coord.lon}`
        this.lat = `Lat: ${data.coord.lat}`
        this.degrees = `${Math.round(data.main.temp)}°C`;
        this.windSpeed = `Wind Speed &#9780;: ${data.wind.speed}km/h`
        this.query = ''
      }
    }
  }
}
</script>

<style lang="scss">
  @mixin rect-theme{
    background: #2D28FF;
    color: white;
    border-radius: 5px;
    outline: none;
    border: 0;
  }
  // // #app{
  // //   display: flex;
  // //   justify-content: center;
  // //   align-items: center;

  // // }

  @media (max-width: 800px){
    *{
      transform: scale(97%);
    }
  }

  .to-center{
    display: flex;
    justify-content: center;
  }

  *{
    font-family: "BPG Nino Mtavruli", sans-serif;
    box-sizing: border-box;
    font: 300
  }

  .input{
    font: 400;
    font-family: "BPG Nino Mtavruli", sans-serif;
    font-size: 15px;
  }

  .container{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 90vh;

    .cont{
      box-shadow: 0 0 4px black;
      background: #4F60FF;
      width: 600px;
      height: 413px;
      border-radius: 5px;

        p{
          color: #fff;
          font-size: 20px;
          text-align: center;
        }
      .inputs{
        display: flex;
        justify-content: center;
        margin-top: 20px;
        input{
          @include rect-theme;
          width: 80%;
          height: 30px;
          padding-left: 10px;
          @extend .input;
          &::placeholder{
            color: #fff;
            opacity: .7;
            @extend .input;
          }
        }
        button{
          @include rect-theme;
          @extend .input;
          width: 10%;
          height: 30px;
          cursor: pointer;
          margin-left: 5px;
          transition: .3s;
          &:hover{
            background: white;
            color: #2D28FF;
          }
        }

      }
      .weather{
        display: flex;
        justify-content: center;
        text-align: center;
        &-el{
          transform: translateY(-20px);

        }

        .empty{
          transform: translateY(50px);
        }
        .notFound{
          color: red;
          transform: translateY(85px);
        }
        .position{
          display: flex;
          // justify-content: center;

          width: 120%;
          transform: translateY(-10px);

        }
        width: 550px;
        height: 267px;
        @include rect-theme;
        padding: 10px;
        font-weight: 100;
        // margin-top: 60px;
        h3{
          font-size: 25px;
        }
        h1{
          font-size: 50px;
        }
        h4{
          margin: 10px;
        }
        .wind{
          font-size: 20px;
        }
      }
    }
  }
</style>
