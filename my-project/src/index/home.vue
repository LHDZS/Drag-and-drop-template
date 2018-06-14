<template>
  <div class="home">
    <div class="HomeTiTle">
      <div class="righttwo">
        <div class="Sdianzhao">
          <i></i>
          <span>颜色设置</span>
        </div>
        <div class="Swcaozuo">
          <span class="Span">背景颜色修改：</span>
          <el-color-picker class="yanse" v-model="color2"></el-color-picker>
        </div>
        <div class="Swcaozuo">
          <span class="Span">标题名称修改：</span>
          <el-input style="width:244px" placeholder="输入标题" :minlength="2" :maxlength="10" :value="name" @blur="Homebiaoti" clearable></el-input>
        </div>
        <div class="Swcaozuo">
          <span class="Span">标题颜色修改：</span>
          <el-color-picker class="yanse" v-model="color4"></el-color-picker>
        </div>
      </div>
    </div>
    <div class="Hotop"></div>
    <div class="Homeimg" ref="mybox">
      <div class="shouye_wrap">
        <div class="shouye" @click="hai">
          <Title class="arre" :title="name" :color="color2" :colorz="color4" v-on:showhide="titsh"></Title>
          <!-- <div class="Swipe">
            <div class="arr" :is="sitem.component" 
              v-for="(sitem, sindex) in swipes" 
              :key="sindex" 
              :swipes="swipes" 
              :ArrClassify="ArrClassify" 
              :Swtype="TypeShowHide" 
              @click.native="Swiclick(sindex)"
              :MerchantId="MerchantId" 
              :Swoptions="Hoptions" 
              v-on:SwShowHide="SwShowHide"
              v-on:indicatorposition="heidian" v-on:autoplay="lunbo" v-on:speed="speed" v-on:SwName="SwName" v-on:Swipequeren="Homequeren"></div>
          </div>
          <div class="Navigation">
            <div class="navarr" :is="nitem.component" 
            v-for="(nitem, nindex) in navigs" 
            :key="nindex" 
            :index="nindex" 
            :navigs="navigs" 
            :ArrClassify="ArrClassify" 
            @click.native="Navclick(nindex)" 
            :MerchantId="MerchantId" 
            :Navtype="TypeShowHide" 
            v-on:naviga="navigas" 
            v-on:NavShowHide="NavShowHide"></div>
          </div>
          <div class="Commoditybar">
            <div class="coomarr" ref="Commarr" 
            :is="citem.component" 
            v-for="(citem, cindex) in Cooms" 
            :key="cindex" 
            :index="cindex" 
            :items="Cooms" 
            @click.native="Commclick(cindex)" 
            :color="color1" 
            :Comtype="TypeShowHide" 
            v-on:AddPush="AddPush" 
            v-on:FlagItems="FlagItems" 
            v-on:biaoti="biaot" 
            v-on:fenlei="fenlei" 
            v-on:digital="digital" 
            v-on:duoxuan="duoxuan" 
            v-on:ComShowHide="ComShowHide"
            v-on:CommHead="CommHead">
            </div>
          </div> -->
          <!-- <Tuozhuai></Tuozhuai> -->
          <draggable :list="tuodong" :move="getdata" @update="datadragEnd" :options="{animation:300,group:'people'}">
            <div class="Drarr" v-for="(item, index) in tuodong"
              :is="item.component"
              :key="index"
              :swipes="swipes" 
              :ArrClassify="ArrClassify" 
              :Swtype="TypeShowHide" 
              @click.native="Swiclick(index)"
              :MerchantId="MerchantId"
              :Swoptions="Hoptions" 
              v-on:SwShowHide="SwShowHide"
              v-on:indicatorposition="heidian" 
              v-on:autoplay="lunbo" 
              v-on:speed="speed" 
              v-on:SwName="SwName" 
              v-on:Swipequeren="Homequeren"
              :navigs="navigs"
              :Navtype="TypeShowHide"
              v-on:naviga="navigas"
              v-on:NavShowHide="NavShowHide"
              :items="Cooms"
              :color="color1"
              :Comtype="TypeShowHide"
              v-on:AddPush="AddPush"
              v-on:FlagItems="FlagItems"
              v-on:biaoti="biaot"
              v-on:fenlei="fenlei"
              v-on:digital="digital"
              v-on:duoxuan="duoxuan"
              v-on:ComShowHide="ComShowHide"
              v-on:CommHead="CommHead"
              ></div>
          </draggable>
          <div class="Hright">
            <el-button @click="tijiao()" class="tijiao" type="primary">提交</el-button>
          </div>
        </div>
      </div>
    </div>
    <div class="HoBottom"></div>
    <div class="Holiebiao">
      <draggable :list="tuozhuai" :move="getdata_two" @start="drag" @end="drop" :options="{animation:300,group:{ name:'people', pull:'clone', put:false },sort:false}">
        <div class="Homedrag" v-for="(item,index) in zujian" :key="index">{{item.name}}</div>
      </draggable>
    </div>
  </div>
</template>

<script>
import Title from '../components/Title'
import Navigation from '../components/Navigation'
import Swipe from '../components/swipe'
import eventBus from '../assets/eventBus'
import Commoditybar from '../components/Commoditybar'
import Tuozhuai from '../components/tuozhuai'
import draggable from 'vuedraggable'
import {GoodsCategory,TemplatePage,MerchantId,Www1,BackEnd} from '../assets/BaseApi'

export default {
  name:'home',
  components: {
    Title,
    Navigation,
    Swipe,
    Commoditybar,
    Tuozhuai,
    draggable
  },
  data () {
    return {
        MerchantId:null,
        tuodong: [{component:'Swipe'},{component:'Navigation'},{component:'Commoditybar'}],
        swipes: [{component:'Swipe'}],
        navigs: [{component:'Navigation'}],
        Cooms:[{component:'Commoditybar'}],
        Pictures:[{component:'Picture'},{component:'Picture'},{component:'Picture'}],
        zujian:[{name:'商品栏'},{name:'轮播图'},{name:'导航栏'}],
        tuozhuai: [{component:'Commoditybar'},{component:'Swipe'},{component:'Navigation'}],
        ind:null,
        TypeShowHide:true,
        delBol: false,
        Prxianyin: true,
        Pranniu: true,
        Pindex:null,
        list:[],
        listee: {
          "flag":[
            {
              tag: '商品栏',
              style: 1,
              flag: '1,2,3,4',
              num: 4,
              classify: '小图',
              type:false,
              classtype:'TAB栏'
            }
          ],

          "flag_display":true,
          "video":{
            display:true,
            style:1,
            height:350,
            page:''
          },
          "banner":{
            display:true,
            logo:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253159871834.jpg',
            name:'商城名称',
            background:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253159764283.png',
            height:120,
            isbig:true,
            style: '1',
            color:'#ffffff'
          },
          "photos":[
            {
              display:true,
              class:'Pimg',
              colstyle:1,
              colindex:0,
              height:368,
              img:[{tpimg:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg',page:'page'}],
            },
            {
              display:true,
              class:'Pimg',
              colstyle:1,
              colindex:0,
              height:368,
              img:[{tpimg:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg',page:'page'}],
            },
            {
              display:true,
              class:'Pimg',
              colstyle:1,
              colindex:0,
              height:368,
              img:[{tpimg:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg',page:'page'}],
            },
          ],
          "menu":[
              {
                name: "\u6210\u529f\u6848\u4f8b",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "article?id=889"
            },
            {
                name: "\u516c\u53f8\u4ecb\u7ecd",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "article?id=888"
            },
            {
                name: "\u7559\u8a00\u7c3f",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "gbook"
            },
            {   
                name: "\u5730\u56fe\u5bfc\u822a",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "map"
            }
          ],
          "discounts":true,
          "menu_display":true,
          "swiper":{
            display:true,
            dots: true,
            loop: true,
            maskStatus: false,
            SwNume:1,
            Swdot:1,
            Swmove:1,
            interval: 3000,
            list: [
              { link: 'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg',
                  name: '山水',
                  page: 'page' },
                { link: 'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg',
                  name: '山水2',
                  page: 'page2' },
                { link: 'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg',
                  name: '山水3',
                  page: 'page3' }
            ]
          },
          "router":{
            category: "categorySearch",
            categoryDetail: "categoryInfoTopScroll"
          },
          "items":[{component:'Commoditybar'}],
          "backgroundColor":"#000",
          "titleColor":"#fff"
        },
        arr: [],
        // APPID
        appid: null,
        // 首页title
        name: '首页',
        titsh: true,
        // 轮播数据
        shuff: true,
        poi: true,
        spe: 3000,
        // 页面分类
        Hoptions :[
          {
              value: 'article?id=',
              label: '单页',
              children: [
              {
                  value: '889',
                  label: '新闻',
              },{
                  value: '888',
                  label: '介绍',
              }
              ],
          },{
              value: 'tiaozhuan',
              label: '跳转',
              children: [
              {
                  value: 'gbook',
                  label: '调查',
              },
              {
                  value: 'map',
                  label: '物流',
              }
              ],
          },{
              value: 'goodsInfo?id=',
              label: '商品详情',
          }
        ],
        //分类
        ArrClassify:[
          {name:'如何通过CSS自动隐藏超出宽度的字不带省略号如何通过CSS自动隐藏超出宽度的字不带省略号', id:1,title:'如何通过CSS自动隐藏超出宽度的字不带省略号如何通过CSS自动隐藏超出宽度的字不带省略号',lingqu:'liji',icon:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15254804789786.jpg'},
          {name:'youhuijuan2',id:2,title:'双列',lingqu:'liji',icon:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15254804847298.jpg'},
          {name:'youhuijuan3',id:3,title:'三列',lingqu:'liji2',icon:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15254804889866.jpg'},
        ], 
        Arrparticular:[
          {name:'如何通过CSS自动隐藏超出宽度的字不带省略号如何通过CSS自动隐藏超出宽度的字不带省略号', id:1,title:'如何通过CSS自动隐藏超出宽度的字不带省略号如何通过CSS自动隐藏超出宽度的字不带省略号',lingqu:'liji',thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15254804789786.jpg'},
          {name:'youhuijuan2',id:2,title:'双列',lingqu:'liji',thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15254804847298.jpg'},
          {name:'youhuijuan3',id:3,title:'三列',lingqu:'liji2',thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15254804889866.jpg'},
        ],
        // 商品列表数据
        shale:[
          {"tag": "商品栏","flag": [1,2,3,4],"num": 4,"style": 1,"classify": '小图',"type":false,"classtype":'TAB栏'}
        ],
        comms:null,
        // 商品默认值
        moren: {"tag": "商品栏","flag": [1,2,3,4],"num": 4,"style": 1,"classify": '小图',"type":false,"classtype":'TAB栏'},
        // 设置默认值
        shuliang:4,
        yangshi:["long","long"],
        flag:[1,2,3,4],
        tag:'商品栏',
        colsh: null,
        // 导航
        navigaz: [
              {
                name: "\u6210\u529f\u6848\u4f8b",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "article?id=889"
            },
            {
                name: "\u516c\u53f8\u4ecb\u7ecd",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "article?id=888"
            },
            {
                name: "\u7559\u8a00\u7c3f",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "gbook"
            },
            {   
                name: "\u5730\u56fe\u5bfc\u822a",
                icon: "http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg",
                page: "map"
            }
        ],
        navsh:null,
        // key:[0],
        // tab选择地址
        commodity: 'categorySearch',
        classify: 'categoryInfoTopScroll',
        // 颜色选择器
        color1: null,
        color2: '#000',
        color4: '#fff',
        type:'',
        IEbanben: null,
    }
  },
  mounted:function () {
    // 计算高度
    var doc = document.documentElement.clientHeight
    this.$refs.mybox.style.height = doc-200 + 'px'
    const that = this;
    window.onresize = function temp() {
        that.$refs.mybox.style.height = `${document.documentElement.clientHeight - 200}px`;
    };

    // 获取URL 地址参数
    var getUrlStr =  function(name) {
  　　var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
  　　var r = window.location.search.substr(1).match(reg);
      　　if(r != null) return unescape(r[2]);
      　　return null;
    }
    console.log('获取URL--type')
    var type = getUrlStr ("type")
    var backid = getUrlStr ("id")
    //   兄弟组件传值
    // 商品
      eventBus.$on("myFun",(message)=>{
            this.classify = message
      })
    //   分类
      eventBus.$on("myRun",(message)=>{
            this.commodity = message
      })
      this.Noimg()
      this.ajax(type,backid)
      this.PageData(backid)
  },
  methods: {
    getdata (evt){
      console.log('托拽中')
      console.log(evt)
    },
    datadragEnd (evt){
      console.log('1111拖完了1')
      // console.log(evt)
    },
    getdata_two (evt){
      console.log('托拽中2')
      console.log(evt)
    },
    drag (e) {
      console.log('拖拽执行drag')
    },
    drop (e) {
      console.log('拖拽执行drop')
      this.tuozhuai = [{component:'Commoditybar'},{component:'Swipe'},{component:'Navigation'}]
    },
    ajax (type,backid) {
      var _this = this
      if (type == 'front') {
        this.type = Www1
        this.TypeShowHide = false
      }else if (type == 'back'){
        this.type = BackEnd+backid
        this.TypeShowHide = true
      }
      console.log(this.type)
      this.$ajax.get(this.type)
      .then(function (res) {
          // 取值
          _this.appid = res.data.ext.extAppid
          var data = res.data.ext.ext
          _this.name = data.title || _this.name
          _this.color4 = data.titleColor || _this.color4
          _this.color2 = data.backgroundColor || _this.color2
          _this.shuff = data.swiper.loop
          _this.poi = data.swiper.dots
          _this.spe = data.swiper.interval
          _this.Cooms = data.items || _this.Cooms
          _this.shale = data.flag || _this.shale
          _this.navigaz = data.menu || _this.navigaz
          _this.commodity = data.router.category || _this.commodity
          _this.classify = data.router.categoryDetail || _this.classify
          _this.listee = data
          if (res.data.ext.extAppid != 0) {
            _this.Prxianyin = data.discounts
            _this.Pranniu = data.discounts
          }else {
            _this.Pranniu = data.discounts
          }
          _this.metIdajax(res.data.ext.extAppid)
      })
      .catch(function (err) {
          console.log(err)
          console.log('get请求失败了')
      });
    },
    // 获取页面下拉菜单数据
    PageData (id) {
        var _this = this
        this.$ajax.get(TemplatePage+id)
        .then(function (res) {
            _this.Hoptions = res.data
        })
        .catch(function (err) {
            console.log(err)
            console.log('页面分类地址请求失败了')
        });
    },
    // 获取 ma id 
    metIdajax (extAppid) {
      var _this = this
      this.$ajax.get(MerchantId + extAppid)
      .then(function (res) {
          _this.MerchantId = res.data
          _this.ClassifyData(res.data)
          console.log('merchant_id请求成功')
      })
      .catch(function (err) {
          console.log(err)
          console.log('merchant_id请求失败了')
      });
    },
    // 商品分类组件数据请求
    ClassifyData (MerchantId) {
      console.log('分类数据请求进来了吗')
        var _this = this
        this.$ajax.get(GoodsCategory, {
            params: {
            merchant_id : MerchantId || 130,
            }
        })
        .then(function (res) {
            _this.ArrClassify = res.data.data.items
            console.log('分类页数据请求数据成功')
        })
        .catch(function (err) {
            console.log(err)
            console.log('分类页数据请求失败了')
        });
    },
    // 视频
    VideoSite(p) {
      this.listee.video.page = p
    },
    Viheight (h) {
      this.listee.video.height = h
    },
    ViShowHide (s) {
      this.listee.video.display = s
    },
    PrShowHide (s) {
      this.listee.discounts = s
    },
    ComShowHide(s) {
      this.listee.flag_display = s
    },
    // 导航
    NavShowHide(s) {
      this.listee.menu_display = s
    },
    // 店招
    SiShowHide (s) {
      this.listee.banner.display = s
    },
    PiShowHide (s,i) {
      this.listee.photos[i].display = s
    },
    // 轮播图显示隐藏
    SwShowHide (s) {
      this.listee.swiper.display = s
    },
    SwName (Bol,i) {
      this.listee.swiper.maskStatus = Bol
      this.listee.swiper.SwNume = i
    },
    Noimg () {
      var Imgs = document.getElementsByTagName('img')
        for (var i=0;i<Imgs.length;i++) {
            Imgs[i].onmousedown = function(e){  
                e.preventDefault()  
            }; 
        }
    },
    Swiclick (i) {
      this.ind = i
      console.log(i+'点击')
    },
    Coluclick (e) {
      this.colsh = e
    },
    Navclick (k) {
      this.navsh = k
    },
    Commclick(h) {
      this.comms = h
    },
    // 图片index获取
    Piclick(i) {
      this.Pindex = i
    },
    hai (e) {
      // 轮播
      var Arr = document.getElementsByClassName("arr")
      // // 导航
      var NavArr = document.getElementsByClassName("navarr")
      // 拖动组件内部
      var DrArr = document.getElementsByClassName("Drarr")
      // // 商品栏
      var Coomarr = document.getElementsByClassName("coomarr")
      // 页面title
      var Rightarr = document.getElementsByClassName('HomeTiTle')
      var Arre = document.getElementsByClassName('arre')

      if(!Arre[0].contains(e.target)){
          Rightarr[0].getElementsByClassName('righttwo')[0].style.display = 'none'
      }else{
          Rightarr[0].getElementsByClassName('righttwo')[0].style.display = 'block'
      }
      var ClickArr = function (Arr,right,sh) {
        for (var index = 0; index < Arr.length; index++) {
          if(Arr[index]){
            if(!Arr[index].contains(e.target)){
                Arr[index].getElementsByClassName(right)[0].style.display = 'none'
            }else{
              if (sh) {
                if (index == sh) {
                  Arr[index].getElementsByClassName(right)[0].style.display = 'block'
                }else {
                  Arr[sh-(sh-index)].getElementsByClassName(right)[0].style.display = 'block'
                }
              }else {
                Arr[index].getElementsByClassName(right)[0].style.display = 'block'
              }
            }
          }
        }
      }
      ClickArr(DrArr,"right",this.ind)
      // 轮播图点击事件
      ClickArr(Arr,"right",this.ind)
      // // 导航点击事件
      ClickArr(NavArr,"right",this.navsh)
      // // 商品栏点击事件
      ClickArr(Coomarr,"right",this.comms)
    },
    // 图片样式
    Ptupian (e,i) {
      this.listee.photos[i].img = e
    },
    Pgeshi (e,i,y) {
      this.listee.photos[i].colstyle = e
      this.listee.photos[i].colindex = y
    },
    Pclas (e,i) {
      this.listee.photos[i].class = e
    },
    Pheight (h,i) {
      this.listee.photos[i].height = h
    },
    // 店招样式
    Syangshi(e,i) {
      this.listee.banner.isbig = e
      this.listee.banner.style = i
    },
    Slogo(e) {
      this.listee.banner.logo = e
    },
    Sbeijing(e) {
      this.listee.banner.background = e
    },
    Sname(e) {
      this.listee.banner.name = e
    },
    Sgaodu(e) {
      this.listee.banner.height = e
    },
    Scolor(c) {
      this.listee.banner.color = c
    },
    // 商品栏样式储存
    fenlei(p,index,x) {
      if(!this.shale[index]){
        this.shale[index] = {}
      }
      this.shale[index].style = p || 1
      this.shale[index].classify = x
    },
    // 商品栏删除
    FlagItems (v,i) {
      this.delBol = true
      this.shale.splice(i, 1, undefined)
      console.log(this.shale)
    },
    // Tab栏商品栏切换
    CommHead (b,v,index) {
      if(!this.shale[index]){
        this.shale[index] = {}
      }
      this.shale[index].type = b
      this.shale[index].classtype = v
    },
    // 商品栏添加
    AddPush (j) {
      this.delBol = false
      this.shale.push(j)
    },
    // 商品显示数量储存
    digital(v,index) {  
      if(!this.shale[index]){
        this.shale[index] = {}
      }
      this.shale[index].num = v
    },
    // 商品标签储存
    duoxuan (y,index) {
      if(!this.shale[index]){
        this.shale[index] = {}
      }
      this.shale[index].flag = y
    },
    // 商品标题储存
    biaot(s,index) {
      console.log('title',index)
      if(!this.shale[index]){
        this.shale[index] = {}
      }
      this.shale[index].tag = s
    },
    // 页面名字储存
    Homebiaoti(e) {
      this.name = e.target.value 
    },
    // 优惠券储存
    youhui(i) {
      // console.log(i)
      if (this.arr.length < 4) {
        this.arr.push(i)
      }else{
        return
      }
      this.list['prefer'] = this.arr;
    },
    // 轮播图是否轮播储存
    lunbo (e,i) {
      this.shuff = e
      this.listee.swiper.Swmove = i
    },
    // 轮播小点是否显示储存
    heidian (e,i) {
      this.poi = e
      this.listee.swiper.Swdot = i
    },
    // 轮播速度
    speed(value) {
      this.spe = value
    },
    Homequeren (list) {
      this.listee.swiper.list = list
    },
    // 导航栏九宫格
    navigas(val) {
      this.navigaz = val
    },
    // 提交
    tijiao() {
      let _this = this
      let normal = {
        tag: '商品栏',
        flag: '1,2,3,4',
        num: 4,
        style: 1,
        classify: '小图',
        type:false,
        classtype:'TAB栏'
      }
      normal = this.moren
      // console.log('======')
      // console.log(normal)
      // 商品列表
      let shale = this.shale.filter( item => {
        return item != undefined
      })
      if(!this.delBol && shale.length == 0){
        this.shale.push(normal)
        this.list['flag'] = shale
      }else{
        for(let i = 0; i < shale.length; i++){
          if(JSON.stringify(shale[i]) == '{}'){
            shale.splice(i, 1, normal)
          }else {
            shale[i].style = shale[i].style || normal.style
            shale[i].flag = shale[i].flag.toString() || normal.flag
            shale[i].num = shale[i].num || normal.num
            shale[i].tag = shale[i].tag || normal.tag
            shale[i].classify = shale[i].classify || normal.classify
            shale[i].type = shale[i].type || normal.type
            shale[i].classtype = shale[i].classtype || normal.classtype
          }
        }
      }
      
      // console.log(shale)
      this.listee.flag = shale
      // console.log('=========')
      // 轮播图
      this.listee.swiper.loop = this.shuff
      this.listee.swiper.dots = this.poi
      this.listee.swiper.interval = this.spe
      // 导航栏
      this.listee.menu = this.navigaz
      //  标题
      this.listee.title = this.name
      this.listee.titleColor = this.color4
      // 颜色
      this.listee.backgroundColor = this.color2

      // tab选择
      this.listee.router.category = this.commodity
      this.listee.router.categoryDetail = this.classify

      // items 储存
      this.listee.items = this.shale
      let datas = []
      for (var key in this.Cooms) {
        if (this.Cooms[key] != 2) {
          datas.push(this.Cooms[key])
        }
        this.listee.items = datas
      }
      // this.listee.items = this.Cooms

      console.log(this.listee)
      this.$ajax.post(this.type, this.listee)
      .then(function (res) {
          console.log(res)
          _this.$message({
            showClose: true,
            message: '提交数据成功',
            type: 'success'
          });
      })
      .catch(function (err) {
          console.log(err);
          _this.$message({
            showClose: true,
            message: '提交数据失败',
            type: 'error'
          });
      });
    },
    // 添加轮播图
    swipe(Swipe) {
      this.swipes.push({
        'component': "Swipe"
      })
    },
    // 添加导航栏
    navig(Navigation) {
      this.navigs.push({
        'component': "Navigation"
      })
    }
  }
}
</script>

<style lang="less">
.home {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
  width: 100%;
  height: 100%;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  background-color: #ecf0f5;
  padding-bottom: auto;
  -ms-overflow-style:none;
  overflow:-moz-scrollbars-none;
}
.Holiebiao {
  width: 100px;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background-color: #ccc;
}
// 公共
.Cbody {
  width: 100%;
  height: 100%;
  border: 1px solid #fff;
  box-sizing: border-box;
}
.Cbody:hover {
    cursor: move;
    border-left: 1px dashed #409EFF;
    border-right: 1px dashed #409EFF;
    border-top: 1px solid #409EFF;
    border-bottom: 1px solid #409EFF;
}
.Stuimg {
    width: 100%;
    height: 100%;
}
.HomeTiTle {
  width: 200px;
  border: 0px solid #fff;
  position: relative;
}
.tabBar {
  width: 100%;
  height: 50px;
  background-color: #000;
  display: flex;
}
.tabdiv {
  flex: 1;
}
.tabimg {
  display: inline-block;
  width: 30px;
  height: 30px;
}
.tabspan {
  display: block;
  font-size: 10px;
  color: #fff;
}
.columns {
  position: relative;
  width: 100%;
  height: 100%;
}
.boFixed {
  font-size: 14px;
  color: #666;
  width: 270px;
  position: fixed;
  left: 455px;
  top: 65px;
  padding: 12px 10px;
  min-height: 28px;
  background: #f8f8f8;
  border-radius: 5px;
  border: 1px solid #d1d1d1;
  z-index: 1999;
}
.bottom {
  font-size: 14px;
  color: #666;
  height: 7%;
  position: absolute;
  left: 39%;
  bottom: 20%;
  padding: 12px 10px;
  min-height: 28px;
  background: #f8f8f8;
  border: 1px solid #d1d1d1;
  z-index: 1999;
}
.Hright {
  font-size: 14px;
  color: #666;
  width: 100%;
  height: 40px;
  position: fixed;
  right: 0;
  bottom: 0;
  min-height: 28px;
  background: #f8f8f8;
  border: 1px solid #d1d1d1;
  z-index: 1999;
}
// input组件
.el-input-number--mini .el-input__inner {
    padding-left: 22px;
    padding-right: 35px;
}
.righttwo {
  display: block;
  font-size: 14px;
  color: #666;
  width: 371px;
  cursor: default;
  position: absolute;
  left: 250%;
  top: 20px;
  padding: 28px 17px 15px 17px;
  text-align: left;
  background: #f8f8f8;
  border-radius: 5px;
  border: 1px solid #d1d1d1;
}
.addition {
  margin: 0 !important;
}
.tishini {
  width: 100%;
  height: 100%;
  text-align: center;
  line-height: 500px;
  font-size: 24px;
  color: #fff;
  position: fixed;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 2888;
}
.array {
    width: 0;
    height: 0;
    border-style: solid;
    border-width: 6px 7px 6px 0;
    border-color: transparent #d1d1d1 transparent transparent;
    position: absolute;
    left: -8px;
    top: 8px;
  }
  .array::after {
    content: "";
    width: 0;
    height: 0;
    border-style: solid;
    border-width: 6px 7px 6px 0;
    border-color: transparent #f8f8f8 transparent transparent;
    position: absolute;
    left: 2px;
    top: -6px;
  }
  .Sdianzhao {
    width: 100%;
    display: block;
    height: 30px;
    margin-bottom: 25px;
}
.Sdianzhao i {
    display: inline-block;
    width: 4px;
    height: 12px;
    background-color: #ad0018;
    margin-top: 9px;
    float: left;
}
.Sdianzhao span {
    font-size: 18px;
    color: #ad0018;
    margin-left: 8px;
    float: left;
}
.Span{
    font-size: 16px;
    margin-right: 15px;
    float: left;
}
.Stuimg {
    width: 100%;
    height: 100%;
}
.Stuspan {
    display: inline-block;
    width: 100%;
    height: 20%;
    line-height: 25px;
    position: absolute;
    bottom: 0px;
    left: 0px;
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
}
.Sshangcheng {
    width: 100%;
    height: 52px;
    line-height: 52px;
}
.Sshangcheng .shangchengname {
    font-size: 16px;
    margin-right: 15px;
}
.Sshangcheng .shurukuang {
    width: 273px;
    height: 38px;
}
.Sshangcheng .yanse {
    margin-top: 10px;
    margin-left: 4px;
    float: left;
}
.Sshangcheng .scnamecolor {
    font-size: 16px;
    margin-right: 15px;
    float: left;
}
.Sshangcheng .sctpgaodu {
    font-size: 16px;
    margin-right: 19px;
    float: left;
}
.zhanwei {
    width: 100px;
    height: 10px;
}
.drag{
    width: 100px;
    height: 100px;
    position: absolute;
    top: 0;
    left: 0;
    background-color: red;
}
.Homedrag {
    width: 50px;
    height: 50px;
    margin: 5px auto;
    line-height: 50px;
    border: 1px solid #fff;
    cursor: move;
}
</style>