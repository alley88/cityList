<template>
  <div class="city-hot">
    <div id="hot">
      <div class="hot">热门城市</div>
      <div class="hot-city-list">
        <div class="city-item" v-for="(item,index) in hotCity" :key="index">{{item.nm}}</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
        <div class="city-item">北京</div>
      </div>
    </div>
    <!-- 城市渲染 -->
    <div ref="list">
      <div class="city" v-for="(item,index) in cityList" :key="index">
        <div class="city-title-letter">{{item.index}}</div>
        <div class="city-list">
          <div class="city-item" v-for="(item,idx) in item.list" :key="idx">{{item.name}}</div>
        </div>
      </div>
      <!-- 序号 -->
      <div class="city_index">
        <div
          v-for="(item,index) in cityList"
          :key="index"
          @click="handleScroll(index)"
        >{{item.index}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "citylist",
  data() {
    return {
      cityList: [],
      hotCity: []
    };
  },
  created() {
    this.$http("/api/cityList").then(res => {
      let data = res.data.data.cities;

      let city = this.formatCityList(data);

      this.cityList = city.cityList;
      this.hotCity = city.hotCity;
      console.log(this.cityList);
    });
  },
  methods: {
    /*
      {id: 1, nm: "北京", isHot: 1, py: "beijing"}
      {id: 10, nm: "上海", isHot: 1, py: "shanghai"}
      {id: 20, nm: "广州", isHot: 1, py: "guangzhou"}
      {id: 30, nm: "深圳", isHot: 0, py: "shenzhen"}
      {id: 42, nm: "西安", isHot: 0, py: "xian"}

      isHot:1 热门城市,
      isHot:0 非热门城市
    */

    //封装数据处理方法
    formatCityList(cities) {
      /* 
      思路一、
        1、返回热门城市 hotCity

        2、城市列表 cityList

      思路二、
        热门城市思路
          根据isHot push到hotCity中

      思路三、
        城市列表数据
        [ 
          {
            index:"A",
            list:[
              {name:"北京",id:10}
              ]
          }
        ]

        首先判断当前当前城市的首字符是否在cityList中，如果存在将当前城市push到list中
        如果不存在创建当前对象，以及将当前城市push到list中
     */

      let hotCity = [];
      let cityList = [];

      //热门城市
      for (var i = 0; i < cities.length; i++) {
        if (cities[i].isHot == 1) {
          hotCity.push(cities[i]);
        }
      }

      //城市列表

      for (var i = 0; i < cities.length; i++) {
        //获取城市的首字符
        let firstLetter = cities[i].py.slice(0, 1).toUpperCase();

        //如果存在获取当cityList的下标然后push进行
        if (tobStop(firstLetter)) {
          for (var j = 0; j < cityList.length; j++) {
            if (firstLetter == cityList[j].index) {
              cityList[j].list.push({ name: cities[i].nm, id: cities[i].id });
            }
          }
        } else {
          //如果不存在则直接push进行
          cityList.push({
            index: firstLetter,
            list: [{ name: cities[i].nm, id: cities[i].id }]
          });
        }
      }

      //判断当前城市首字符是否在cityList中存在如果存在返回true  没有返回false
      function tobStop(firstLetter) {
        for (var j = 0; j < cityList.length; j++) {
          if (firstLetter == cityList[j].index) {
            return true;
          }
        }
        return false;
      }

      //城市排序
      cityList.sort((a, b) => {
        if (a.index > b.index) {
          return 1;
        } else {
          return -1;
        }
      });

      return {
        cityList,
        hotCity
      };
    },
    handleScroll(index) {
      let indexLists = this.$refs.list.querySelectorAll(".city-title-letter");
     
      this.$refs.list.parentNode.scrollTop = indexLists[index].offsetTop;
       console.log(this.$refs.list.parentNode.scrollTop,indexLists[index].offsetTop)
    }
  }
};
</script>


<style scoped >
.city-hot {
  background: #ebebeb;
  height: 100%;
  color: #333;
  overflow:auto;


  width: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
}
.hot {
  padding-left: 0.3rem;
  height: 0.6rem;
  font-size: 0.28rem;
  line-height: 0.6rem;
}
.hot-city-list {
  background: #f5f5f5;
  padding-left: 0.3rem;
  display: flex;
  flex-wrap: wrap;
  padding-bottom: 0.16rem;
  margin-right: 0.4rem;
}
.hot-city-list > .city-item {
  width: 1.9rem;
  height: 0.6rem;
  text-align: center;
  line-height: 0.6rem;
  color: #333;
  background: #fff;
  border: 1px solid #e6e6e6;
  font-size: 0.28rem;
  margin-top: 0.3rem;
  margin-right: 0.26rem;
}
.city-title-letter {
  padding-left: 0.5rem;
  height: 0.6rem;
  line-height: 0.6rem;
}
.city-list {
  background: #f5f5f5;
  margin-right: 0.4rem;
}
.city > .city-list > .city-item {
  height: 0.88rem;
  padding-left: 0.3rem;
  line-height: 0.88rem;
  border-bottom: 1px solid #c8c7cc;
  font-size: 0.28rem;
}
/*  */

.city_index {
  position: fixed;
  right: 10px;
  top: 20px;
}
.city_index > div {
  padding: 10px 0;
}
</style>
