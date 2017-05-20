<template>
  <div class="goods">
    <div class="menu-wrapper" v-el:menu-wrapper>
      <ul>
        <li v-for="item in goods" class="menu-item" :class="{'current':currentIndex===$index}"
            @click="selectMenu($index)">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" v-el:foods-wrapper>
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img width="57" height="64" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">${{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">${{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice"
              :min-price="seller.minPrice"></shopcart>
  </div>
</template>
<style>
  .goods {
    position: absolute;
    display: flex;
    overflow: hidden;
    width: 100%;
    top: 174px;
    bottom: 46px;
  }

  .goods .menu-wrapper {
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
  }

  .goods .foods-wrapper {
    flex: 1;
  }

  .menu-wrapper .menu-item {
    display: table;
    width: 56px;
    height: 54px;
    line-height: 14px;
    padding: 0 12px;
  }

  .current {
    position: relative;
    z-index: 10;
    margin-top: -1px;
    background: #fff;
    font-weight: 700;
  }

  .current .text {

  }

  .text .icon {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    height: 12px;
    margin-right: 2px;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }

  .text .decrease {
    background-image: url("decrease_3@2x.png");
  }

  .text .discount {
    background-image: url("discount_3@2x.png");
  }

  .text .guarantee {
    background-image: url("guarantee_3@2x.png");
  }

  .text .invoice {
    background-image: url("invoice_3@2x.png");
  }

  .text .special {
    background-image: url("special_3@2x.png");
  }

  .menu-item .text {
    font-size: 12px;
    display: table-cell;
    width: 56px;
    vertical-align: middle;
  }

  .foods-wrapper .title {
    padding-left: 14px;
    height: 26px;
    line-height: 26px;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147, 153, 159);
    background: #f3f5f7;
  }

  .foods-wrapper .food-item {
    display: flex;
    margin: 18px;
  }

  .food-item .icon {
    flex: 0 0 57px;
    margin-right: 10px;
  }

  .food-item .content {
    flex: 1;
  }

  .content .name {
    margin: 2px 0 8px 0;
    height: 8px;
    line-height: 8px;
    font-size: 14px;
    color: rgb(7, 17, 27);
  }

  .content .desc {
    margin-bottom: 8px;
    line-height: 12px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .content .extra {
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);

  }

  .extra .count {
    margin-right: 12px;
  }

  .content .price {
    font-weight: 700;
    line-height: 24px;
  }

  .price .now {
    margin-right: 8px;
    font-size: 14px;
    color: red;;
  }

  .price .old {
    text-decoration: line-through;
    font-size: 10px;
    color: darkgray;
  }

  .cartcontrol-wrapper {
    position: absolute;
    right: 16px;
    width: 20px;
  }
</style>
<script>
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          })
        })
        return foods;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === 0) {
          this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
        }
      });
    },
    methods: {
      selectMenu(index) {
        let foodlist = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodlist[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      _initScroll() {
        this.menuScroll = new BScroll(this.$els.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
          click: true,
          probeType: 3
        });

        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calculateHeight() {
        let foodlist = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodlist.length; i++) {
          let item = foodlist[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      }
    },
    components: {
      shopcart, cartcontrol
    }
  }
</script>
