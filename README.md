# citylist

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```


###思路
```
    {id: 1, nm: "北京", isHot: 1, py: "beijing"}
    {id: 10, nm: "上海", isHot: 1, py: "shanghai"}
    {id: 20, nm: "广州", isHot: 1, py: "guangzhou"}
    {id: 30, nm: "深圳", isHot: 0, py: "shenzhen"}
    {id: 42, nm: "西安", isHot: 0, py: "xian"}

    isHot:1 热门城市,
    isHot:0 非热门城市


   
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



