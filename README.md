# how-use-baiduAPI-of-Vue
如何使用vueBaidu API 在项目中进行地理位置搜索和红点定位/经纬度定位


# 在vue项目中使用baidu地图API


### 安装


[baiduAPI of vue](https://github.com/Dafrok/vue-baidu-map)


首先在项目中安装依赖


```
npm install --save vue-baidu-map
```


在main.js 引入依赖


```
import BaiduMap from 'vue-baidu-map'

Vue.use(BaiduMap, {
  ak: '秘钥XX'    //去官网申请ak密钥
})
```


### 使用文档


[❤贴心文档，详细的讲解](https://dafrok.github.io/vue-baidu-map/#/zh/start/installation)



### 案例


vue中


```
<el-form-item label="详细地址" prop="areaDetail">
    <div class="mapStyle">
    <el-input placeholder="请输入详细地址" style="margin-left: 0" class="inputWidth" v-model="insertStoreFrom.areaDetail"></el-input>
    <baidu-map class="map" :center="center" :zoom="zoom" @ready="handler" :scroll-wheel-zoom="true"   @moving="syncCenterAndZoom"
                @moveend="syncCenterAndZoom"
                >
        <bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" :showAddressBar="true" :autoLocation="true"></bm-geolocation>
        <bm-marker :position="position" :dragging="true" @dragging="draggingMarker"></bm-marker>
        <bm-view class="map"></bm-view>
        <bm-local-search :keyword="insertStoreFrom.areaDetail" :auto-viewport="true" :panel="false" :selectFirstResult="true" @searchcomplete="backSearch"></bm-local-search>
    </baidu-map>
    <div class="mapInputStyle">
        <div class="mapInputStyle_one">
        <div>经度</div>
        <input v-model.number="center.lng" class="mapInputStyle_input">
        </div>
        <div class="mapInputStyle_one">
        <div>维度</div>
        <input v-model.number="center.lat" class="mapInputStyle_input">
        </div>
    </div>
    </div>
</el-form-item>
```


模块js中


data 数据


```
//  百度地图
    center: {lng: 0, lat: 0},
    zoom: 3,
    position:{lng: 0, lat: 0},
    location:'北京',
    keyword:'北京',
```


methods


```
handler ({BMap, map}) {
    console.log(BMap, map)
    this.center.lng = 116.404;
    this.center.lat = 39.915;
    this.position.lng = 116.404;
    this.position.lat = 39.915;
    this.zoom = 15
},
syncCenterAndZoom (e) {
    const {lng, lat} = e.target.getCenter()
    this.center.lng = lng;
    this.center.lat = lat;
    this.position.lng = lng;
    this.position.lat = lat;
    this.zoom = e.target.getZoom()
},
//拖拽红色标点的事情，反填输入框（此拖拽效果不佳）
draggingMarker(e){
    // console.log(e);
    this.center.lng = e.target.point.lng;
    this.center.lat = e.target.point.lat;
},
//检索后返回的数据信息
backSearch(e){
    // console.log(e)
},
```



### 讲解


页面中 baidu-map  为地图元素，样式  width为100%,height为定值


center 为定位数据，文档说可以为对象（经纬度）/字符串（详细地址），测试-只有对象有效，文字地址再测试


zoom   为缩放


moving/moveend   当改变输入框的经纬度的时候，进行定位


bm-geolocation   定位


bm-marker   红色标记点  ，如果拖动红色标点，draggingMarker 函数将返回拖拽位置，这里需要复填经纬度的输入框


bm-view and bm-local-search   中文搜索匹配位置进行定位


这里使用的是keyword来进行定位


:panel="false"，禁止打开搜索的结果框，第一真的很丑，第二没地方放


:selectFirstResult="true" ， 直接选取返回的第一个结果，进行定位


@searchcomplete="backSearch"   当筛选结果定位结束后，会返回信息，本来打算再把定位的信息复填到经纬度输入框里的，发现自动填了




