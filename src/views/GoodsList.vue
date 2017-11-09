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
          <a href="javascript:void(0)" class="price" v-bind:class="{'sort-up':sortFlag}" @click="sortGoods()">Price
            <svg class="icon icon-arrow-short">
              <use xlink:href="#icon-arrow-short"></use>
            </svg>
          </a>
          <a href="javascript:void(0)" class="filterby stopPop" @click.stop="showFilterPop">Filter by</a>
        </div>
        <div class="accessory-result">
          <!-- filter -->
          <div class="filter stopPop" id="filter" v-bind:class="{'filterby-show':filterBy}">
            <dl class="filter-price">
              <dt>Price:</dt>
              <dd><a href="javascript:void(0)" @click="priceChecked = 'all'"
                     v-bind:class="{'cur':priceChecked=='all'}">All</a></dd>
              <dd v-for="(item,index) in priceFilter">
                <a href="javascript:void(0)" @click="priceChecked = index"
                   v-bind:class="{'cur':priceChecked==index}">{{item.startPrice}} - {{item.endPrice}}</a>
              </dd>
            </dl>
          </div>

          <!-- search result accessories list -->
          <div class="accessory-list-wrap">
            <div class="accessory-list col-4">
              <ul>
                <li v-for="item in goodsList">
                  <div class="pic">
                    <a href="#"><img :src="'static/'+item.productImg" alt=""></a>
                  </div>
                  <div class="main">
                    <div class="name">{{item.productName}}</div>
                    <div class="price">{{item.productPrice | currency('$')}}</div>
                    <div class="btn-area">
                      <a href="javascript:;" class="btn btn--m">加入购物车</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
            <div class="view-more-normal"
                 v-infinite-scroll="loadMore"
                 infinite-scroll-disabled="busy"
                 infinite-scroll-distance="20">
              <img src="./../assets/loading-spinning-bubbles.svg" v-show="loading">
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--<modal v-bind:mdShow="mdShow" v-on:close="closeModal">-->
    <!--<p slot="message">-->
    <!--请先登录,否则无法加入到购物车中!-->
    <!--</p>-->
    <!--<div slot="btnGroup">-->
    <!--<a class="btn btn&#45;&#45;m" href="javascript:;" @click="mdShow = false">关闭</a>-->
    <!--</div>-->
    <!--</modal>-->
    <!--<modal v-bind:mdShow="mdShowCart" v-on:close="closeModal">-->
    <!--<p slot="message">-->
    <!--<svg class="icon-status-ok">-->
    <!--<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#icon-status-ok"></use>-->
    <!--</svg>-->
    <!--<span>加入购物车成!</span>-->
    <!--</p>-->
    <!--<div slot="btnGroup">-->
    <!--<a class="btn btn&#45;&#45;m" href="javascript:;" @click="mdShowCart = false">继续购物</a>-->
    <!--<router-link class="btn btn&#45;&#45;m btn&#45;&#45;red" href="javascript:;" to="/cart">查看购物车</router-link>-->
    <!--</div>-->
    <!--</modal>-->
    <div class="md-overlay" v-show="overLayFlag" @click.stop="closePop"></div>
    <nav-footer></nav-footer>
  </div>
</template>


<script>
  import NavHeader from '@/components/NavHeader'
  import NavFooter from '@/components/NavFooter'
  import NavBread from '@/components/NavBread'
  import axios from 'axios'

  export default {
    data() {
      return {
        goodsList: [],
        priceFilter: [
          {
            startPrice: '0.00',
            endPrice: '500.00'
          },
          {
            startPrice: '500.00',
            endPrice: '1000.00'
          },
          {
            startPrice: '1000.00',
            endPrice: '1500.00'
          }
        ],
        priceChecked: 'all',
        filterBy: false,
        overLayFlag: false
      }
    },
    components: {
      NavHeader,
      NavFooter,
      NavBread
    },
    mounted: function () {
      this.getGoodsList()
    },
    methods: {
      getGoodsList() {
        axios.get('/goods').then((result) => {
          let res = result.data;
          this.goodsList = res.result
        })
      },
      showFilterPop() {
        this.filterBy = true;
        this.overLayFlag = true;
      },
      setPriceFilter(item) {
        this.priceChecked = item;
      },
      closePop() {
        this.filterBy = false;
        this.overLayFlag = false;
      }
    }
  }
</script>

<style>
  @import "../assets/css/base.css";
  @import "../assets/css/product.css";
</style>
