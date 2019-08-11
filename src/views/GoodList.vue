<template>
  <div>
    <nav-header></nav-header>
    <nav-bread>
      <span>Goods</span>
    </nav-bread>
    <div class="accessory-result-page accessory-page">
      <div class="container">
        <div class="filter-nav">
          <span class="sortby">Sort by:</span>
          <a href="javascript:void(0)" class="default cur">Default</a>
          <a href="javascript:void(0)" class="price">Price <svg class="icon icon-arrow-short"><use xlink:href="#icon-arrow-short"></use></svg></a>
          <a href="javascript:void(0)" class="filterby stopPop" @click="showFilter">Filter by</a>
        </div>
        <div class="accessory-result">
          <!-- filter -->
          <div class="filter stopPop" id="filter" :class="{'filterby-show': filterFlag}">
            <dl class="filter-price">
              <dt>Price:</dt>
              <dd><a href="javascript:void(0)" :class="{'cur': priceChecked === 'all'}" @click="setPriceFilter('all')">All</a></dd>
              <dd v-for="(item, index) in priceFilter" :key="index">
                <a href="javascript:void(0)" :class="{'cur': priceChecked === index}" @click="setPriceFilter(index)">{{item.startPrice}} - {{item.endPrice}}</a>
              </dd>
            </dl>
          </div>

          <!-- search result accessories list -->
          <div class="accessory-list-wrap">
            <div class="accessory-list col-4">
              <ul>
                <li v-for="(good, index) in goodList" :key="index">
                  <div class="pic">
                    <a href="#"><img v-lazy="'/static/' + good.prodcutImg"></a>
                  </div>
                  <div class="main">
                    <div class="name">{{ good.productName }}</div>
                    <div class="price">{{ good.prodcutPrice }}</div>
                    <div class="btn-area">
                      <a href="javascript:;" class="btn btn--m">加入购物车</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
            <!-- <div class="view-more-normal"
                v-infinite-scroll="loadMore"
                infinite-scroll-disabled="busy"
                infinite-scroll-distance="20">
              <img src="./../assets/loading-spinning-bubbles.svg" v-show="loading">
            </div> -->
          </div>
        </div>
      </div>
    </div>
    <div class="md-overlay" v-show="layerFlag" @click.stop="closePop"></div>
    <nav-footer></nav-footer>
  </div>
</template>

<script>
import NavHeader from 'components/NavHeader.vue'
import NavFooter from 'components/NavFooter.vue'
import NavBread from 'components/NavBread.vue'
import axios from 'axios'
export default {
  components: {
    NavHeader,
    NavFooter,
    NavBread
  },
  data() {
    return {
      goodList: [],
      priceFilter: [
        {
          startPrice: '0.00',
          endPrice: '100.00'
        },
        {
          startPrice: '100.00',
          endPrice: '500.00'
        },
        {
          startPrice: '500.00',
          endPrice: '1000.00'
        },
        {
          startPrice: '1000.00',
          endPrice: '5000.00'
        }
      ],
      priceChecked: 'all',
      filterFlag: false,
      layerFlag: false
    }
  },
  mounted() {
    this._getGoodsData()
  },
  methods: {
    _getGoodsData() {
      axios.get('/goods').then((result) => {
        var res = result.data
        this.goodList = res.result
      })
    },
    setPriceFilter(index) {
      this.priceChecked = index
      this.closePop()
    },
    showFilter() {
      this.filterFlag = true
      this.layerFlag = true
    },
    closePop() {
      this.filterFlag = false
      this.layerFlag = false
    }
  }
}
</script>

<style>

</style>
