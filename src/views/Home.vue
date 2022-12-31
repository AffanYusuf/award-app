<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <!-- <div class="list-group-wrapper"> -->
        <center>
        <transition name="fade">
          <div class="loading" v-show="loading">
            <b-icon icon="arrow-clockwise" animation="spin" font-scale="4"></b-icon>
          </div>
        </transition>
      </center>
        <div class="row mb-12" id="infinite-list">
          <div class="col-md-12 mt-12" v-for="award in awards" :key="award.id">
            <CardAward :award="award"/>
          </div>
        </div>
      <!-- </div> -->
    </div>


    <b-modal id="modal-1" title="Filter" hide-footer no-close-on-backdrop>
      <label for="range-1">Point Needed</label>
      <div class="row">
        <div class="col-md-2 mt-2">
          Min: <span style="color:dodgerblue"><b>IDR {{ formatPoint(rangePointMinimal) }}</b></span>
        </div>
        <div class="col-md-8 mt-8">
          <b-form-input id="range-1" v-model="rangePointMinimal" type="range" min="0" max="500000"></b-form-input>
        </div>
        <div class="col-md-2 mt-2">
          Max: <span style="color:dodgerblue"><b>IDR {{ formatPoint(rangePointMax) }}</b></span>
        </div>
      </div>
      <br>
      <br>
      <b-form-group
        label="Awards Type"
        v-slot="{ ariaDescribedby }"
      >
        <b-form-checkbox-group
          v-model="selected"
          :options="options"
          :aria-describedby="ariaDescribedby"
          name="flavour-2a"
          stacked
        ></b-form-checkbox-group>
      </b-form-group>
      <br>
      <b-button @click="hideModal(); fetchData()" block variant="primary">Filter</b-button>
    </b-modal>
    
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import CardAward from "@/components/CardAward.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Navbar,
    CardAward,
  },
  data() {
    return {
      awards: [],
      loading: false,
      currentPage: 1,
      totalPages:0,
      limit: 4,
      rangePointMinimal: '10000',
      rangePointMax: '500000',
      selected: [],
        options: [
          { text: 'All Types', value: 'ALL' },
          { text: 'Vouchers', value: 'VOUCHERS' },
          { text: 'Products', value: 'PRODUCTS' },
        ]
    };
  },
  methods: {
    setAwards(data) {
      if (this.awards) {
        this.awards = [...data, ...this.awards];
      }else{
        this.awards = data;
      }
    },
    formatPoint(value) {
      return (value/1).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    },
    hideModal() {
      this.awards = [];
      this.currentPage = 1,
      this.totalPages = 0,
      this.$bvModal.hide('modal-1')
    },
    async fetchData() {
      this.loading = true;
      const params = {
        pointFrom: this.rangePointMinimal,
        pointTo: this.rangePointMax,
        type: this.selected,
        page: this.currentPage,
        limit: this.limit,

      };
      
      const awards = await axios
      .get("http://localhost:5000/award-service/awards", {params});
      this.totalPages = awards.data.metadata.totalPages;
      this.setAwards(awards.data.data);
      setTimeout(() => {
        this.loading = false
      }, 500);
    }
  },
  async mounted() {
    if (!localStorage.getItem('token')) {
      this.$router.push({ name: "Login"})
    }else {
      const body = document.getElementsByTagName('body')[0];
      body.onscroll = async () => {
        if(body.scrollTop + body.clientHeight >= body.scrollHeight) {
          if (this.totalPages != 0 && this.currentPage < this.totalPages && !this.loading) {
            this.currentPage += 1;
            await this.fetchData();
          }
        }
      };

      await this.fetchData();
    }
  },
};
</script>

<style scoped>

  .loading {
    text-align: center;
    position: absolute;
    color: #fff;
    z-index: 9;
    background: dodgerblue;
    border-radius: 5px;
    left: calc(50% - 45px);
    top: calc(50% - 18px);
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }
</style>