
<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>

<div class="mb-12">项目背景及意义</div>

<div class="text-lg">本项目主要的研究对象为2016年8月份上海市10w条共享单车骑行数据，研究目的是为了构建一个提供运营决策的可视化共享单车后台管理系统。该系统使用云函数、云数据库等云服务结合前端框架react，使用GIS特有的空间分析的能力，构建一个前后端一体化的单车管理系统。实现了<span class="font-black text-green-500">单车运营区域划分、订单状态查看、骑行路线动态图、每日骑行时间和骑行距离折线和柱状统计图、每日热点区域查看、每日热点区域词云图、订单散点图、订单统计表格</span>等功能。最终结果产物为展示提供运营决策的可视化图表网站。具有加强城市共享单车的运营区管理、提髙城市空间利用效率、提升城市交通运行水平和增加城市道路承载力等实际意义。</div>

Note: 想到做这个项目的 idea 来源于生活，生活中使用共享单车时总是突然骑出运营区之外，目的地又无停车点，没办法停车。这都时单车运营管理的不当，所以做此系统的初衷就是为了运营管理人员的对单车运营区域管理的决策辅助作用。

---

<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>

<div class="mb-12">数据准备</div>

<ul class="text-2xl">
  <li>从 <a href="https://www.heywhale.com/mw/dataset/5d315ebbcf76a60036e565bf/content" target="_blank">ModelWhale</a> 下载2016年8月份上海市10w条共享单车骑行数据（csv格式的文件）</li>
  <li>初始数据如下：<img src="https://img.alicdn.com/imgextra/i4/O1CN01rOe5zr1R2Jnz4qy7A_!!6000000002053-2-tps-2964-754.png" /></li>
  <li>上海市各个行政区的geoJSON文件</li>
</ul>

---

<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>

<div id="left">

<div class="mb-12">数据预处理</div>

<ul class="text-xl">
  <li>把日期格式的时间处理成时间戳，并且增加字段</li>
  <li>将轨迹字符串根据“ # ”和“ , ”字符，分割成经纬度二维数组</li>
  <li>使用百度地图web API服务——逆地理编码API将起始地和目的地的经纬度转化为地理地址信息（包含地址、周边商区、周边POI信息等）</li>
  <li>计算行驶路线的长度</li>
  <li>利用turf.js分别计算行政区多边形内的点集，然后通过点集合生成外包多边形，最后结合行政区多边形求交集得出运营区多边形</li>
  <li>统计每日的热点地区有哪些和每日每时的骑行人数、骑行距离，并且生成json文件</li>
</ul>

</div>

<div id="right">

```json
{
  "_id"(唯一标识符): "ad20683160126a8202163a3b1b5b70b5",
  "orderid"(订单id): 31355,
  "bikeid"(单车id): 79699,
  "userid"(用户id): 759,
  "start_time"(开始骑行的时间): "2016/8/1 0:23",
  "start_location_x"(开始骑行位置的经度): 121.52,
  "start_location_y"(开始骑行位置的纬度): 31.309,
  "end_time"(结束骑行的时间): "2016/8/1 0:32",
  "end_location_x"(结束骑行位置的经度): 121.525,
  "end_location_y"(结束骑行位置的纬度): 31.316,
  "track"(骑行轨迹路线的经纬度数组): [
    [121.52, 31.309],
    [121.521, 31.309],
    [121.522, 31.309],
    [121.524, 31.309]
  ],
  "start_time_num"(开始骑行的时间戳): 1469982180000,
  "end_time_num"(结束骑行的时间戳): 1469982720000,
  "start_location"(开始骑行的逆地理编码后的位置): {
    "location"(经纬度): { "lng": 121.53088680363962, "lat": 31.31285047602628 },
    "formatted_address"(地址信息): "上海市杨浦区政立路6弄-15号",
    "business"(周围的商圈): "五角场,中原,黄兴公园",
    "addressComponent": {
      "country"(所在的国家): "中国",
      "country_code"(国家code): 0,
      "country_code_iso"(国家code iso): "CHN",
      "country_code_iso2"(国家code iso2): "CN",
      "province"(所在的省份): "上海市",
      "city"(所在的城市): "上海市",
      "city_level"(城市的等级): 2,
      "district"(所在的行政区): "杨浦区",
      "town"(所在的城镇): "",
      "town_code"(城镇code): "",
      "adcode"(邮编): "310110",
      "street"(所在的街道): "政立路",
      "street_number"(街道编号): "6弄-15号",
      "direction"(方向): "东",
      "distance"(距离): "55"
    },
    "pois"(周边的poi信息): [],
    "roads"(周边的道路信息): [],
    "poiRegions": [],
    "sematic_description": "",
    "cityCode"(城市code): 289
  },
  "end_location": {
    "location": { "lng": 121.53585260377156, "lat": 31.319934319202233 },
    "formatted_address": "上海市杨浦区嫩江路1185号新16-17号",
    "business": "中原,五角场,黄兴公园",
    "addressComponent": {
      "country": "中国",
      "country_code": 0,
      "country_code_iso": "CHN",
      "country_code_iso2": "CN",
      "province": "上海市",
      "city": "上海市",
      "city_level": 2,
      "district": "杨浦区",
      "town": "",
      "town_code": "",
      "adcode": "310110",
      "street": "嫩江路",
      "street_number": "1185号新16-17号",
      "direction": "南",
      "distance": "80"
    },
    "pois": [],
    "roads": [],
    "poiRegions": [],
    "sematic_description": "",
    "cityCode": 289
  },
  "distance"(骑行路线的总长度): "910m"
}
```

</div>

Note: 以上数据预处理均使用 node.js 处理数据

---

<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>

<div class="mb-12">数据操作层的处理</div>

<ul class="text-xl">
  <li>将数据预处理后的json文件存储到云数据库中，生成bike集合</li>
  <li>创建user集合</li>
  <li>编写CRUD的云函数</li>
</ul>

<div id="left">

<img src="https://img.alicdn.com/imgextra/i1/O1CN01HGkwAr1K7d5SrSLyd_!!6000000001117-2-tps-862-260.png" style="height: 200px;" class="object-fill" />

</div>

<div id="right">

<img src="https://img.alicdn.com/imgextra/i2/O1CN01GrW5kS1elk5j9Il0w_!!6000000003912-2-tps-862-324.png" style="height: 200px;" class="object-fill" />

</div>

---

<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>

<div class="mb-12">前端架构</div>

<img src="https://img.alicdn.com/imgextra/i3/O1CN011Js2Uh29XJhiFCjw2_!!6000000008077-2-tps-854-436.png" style="height: 500px;" class="pl-16" />

---

<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>

<div class="mb-12">主要功能</div>

<img src="https://img.alicdn.com/imgextra/i1/O1CN01niRJbo22mFIwPPDbR_!!6000000007162-2-tps-832-716.png" style="height: 500px;" class="pl-48" />

---

<!-- 背景图片：中南林业科技大学logo -->
<div style="background-image: url(https://img.alicdn.com/imgextra/i3/O1CN01in9JUc1Vqw2UdKe21_!!6000000002705-2-tps-276-276.png); border-radius: 50%;" class="w-96 h-96 p-2.5 bg-no-repeat bg-cover bg-clip-content opacity-10 absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 -z-999"></div>
