<template>
  <div class="hello">
    <div class="deal border">
      <div class="row border-bottom">
        <h4 class="col title p-2 mx-3 my-1">Deal Of The Day</h4>
      </div>
      <span class="row">
        <div v-for="deal in deals" :key="deal.id" class="col-12 col-lg-6">
          <div class="row">
            <span class="col-6 p-3">
              <img
                class="deal-img"
                :src="require(`@/assets/${deal.image}`)"
                alt="img"
              />
            </span>
            <span class="col-6 py-3">
              <p class="describtion py-2 mt-1">
                Lorem ipsum dolor sit, amet consectetur.
              </p>
              <h6 class="cost">
                <span>{{ deal.opacityCost }}</span>
                ${{ deal.cost }}
              </h6>
              <div class="timer">
                <div class="d-flex text-center">
                  <span class="min-width border py-2 m-1">
                    <p class="number">{{ displayDays }}</p>
                    <p class="text">Days</p>
                  </span>
                  <span class="min-width border py-2 m-1">
                    <p class="number">{{ displayHours }}</p>
                    <p class="text">hours</p>
                  </span>
                  <span class="min-width border py-2 m-1">
                    <p class="number">{{ displayMinutes }}</p>
                    <p class="text">Mins</p>
                  </span>
                  <span class="min-width border py-2 m-1">
                    <p class="number">{{ displaySeconds }}</p>
                    <p class="text">Secs</p>
                  </span>
                </div>
              </div>
              <div class="btn-group mx-auto pt-3">
                <button type="button" :class="deal.btnClass">
                  {{ deal.button }}
                </button>
              </div>
            </span>
          </div>
        </div>
      </span>
    </div>
  </div>
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css"
    rel="stylesheet"
    integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3"
    crossorigin="anonymous"
  />
</template>
<script>
import axios from "axios";
const baseURL = "http://localhost:3000/deals";

export default {
  name: "CountDown",
  props: ["year", "month", "date", "hour", "minute", "second", "millisecond"],
  // props: {
  //   year: {
  //     type: Number,
  //     required: true,
  //   },
  //   month: {
  //     type: Number,
  //     required: true,
  //   },
  //   date: {
  //     type: Number,
  //     required: true,
  //   },
  //   hour: {
  //     type: Number,
  //     required: true,
  //   },
  //   minute: {
  //     type: Number,
  //     required: true,
  //   },
  //   second: {
  //     type: Number,
  //     required: true,
  //   },
  //   millisecond: {
  //     type: Number,
  //     required: true,
  //   },
  // },
  data: () => ({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0,
    registration: "",
    deals: [],
  }),
  computed: {
    _seconds: () => 1000,
    _minutes() {
      return this._seconds * 60;
    },
    _hours() {
      return this._minutes * 60;
    },
    _days() {
      return this._hours * 24;
    },
    end() {
      return new Date(
        this.year,
        this.month,
        this.date,
        this.hour,
        this.minute,
        this.second,
        this.millisecond
      );
    },
  },
  mounted() {
    this.showRemaining();
  },
  methods: {
    formatNum: (num) => (num < 10 ? 0 + num : num),
    showRemaining() {
      const timer = setInterval(() => {
        const now = new Date();
        // const end = new Date(2023, 4, 10);
        const distance = this.end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          return;
        }

        const days = Math.floor(distance / this._days);
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);
        this.displaySeconds = this.formatNum(seconds);
        this.displayMinutes = this.formatNum(minutes);
        this.displayHours = this.formatNum(hours);
        this.displayDays = this.formatNum(days);
        // console.dir(this.displayMinutes);
      }, 1000);
    },
  },
  async created() {
    try {
      const res = await axios.get(baseURL);

      this.deals = res.data;
      this.registration = res.data.registration;
      console.log(res.data[0].registration);
    } catch (e) {
      console.error(e);
    }
  },
};
</script>

<style scoped>
.row {
  --bs-gutter-x: 0rem;
}
.deal {
  font-family: "lora", Arial, Helvetica, sans-serif;
}
p {
  margin: 0;
}
.describtion {
  line-height: 1.286rem;
  font-size: 1.07rem;
}
.timer span {
  line-height: 1.143rem;
}
.deal-img {
  height: 100%;
  font-family: "lora", Arial, Helvetica, sans-serif;
}
.title {
  font-size: 1.286rem;
  line-height: 1.286rem;
}
.number {
  font-weight: 500;
}
.text {
  font-size: smaller;
  font-weight: 600;
  opacity: 0.8;
}
.min-width {
  min-width: 3.145rem;
}
.cost span {
  opacity: 0.9;
}
</style>
