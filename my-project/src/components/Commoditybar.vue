<template>
  <div class="Commoditybar" v-if="ComShowHide" ref="Commarr">
      <div class="Cbody" v-on:mouseenter="dataDetails()" v-on:mouseleave="hiddenDetail()">
        <el-button id="shanchu" v-if="bol" type="text" @click="open2"><i class="el-icon-error col"></i></el-button>
        <div class="Commtitle" v-if="head"> 
            <i class="Commi"></i>
            <span>{{biaoti ? biaoti : '标题不能为空'}}</span>
        </div>
        <div class="CommUl" v-if="!head">
            <div class="Coomli" :class="Tabindex == index ? 'Coomred' : ''" v-for="(item,index) in Commfenlei" :key="index" @click="TabComm(index)">{{item.name}}</div>
            <div class="clear"></div>
        </div>
        <div v-if="Commswitch == 1">
            <div class="henglie" v-for="(item,key) in list" :key="key" v-if="key<shul">
                <img class="hengimg" :src="item.thumb" alt="">
                <div class="hengright">
                    <span class="hengname">{{item.name}}</span>
                    <div class="hengjiage">
                        <span :style="{color:color}"><i class="yangtoufu">￥</i><i class="jiaf">{{item.price}}</i></span>
                    </div>
                    <div class="subscribe">预约</div>
                </div>
            </div>
        </div>
        <!-- 竖二栏 -->
        <div  v-if="Commswitch == 2">
            <div class="henglie_two" v-for="(item,key) in list" :key="key" v-if="key<shul">
                <img class="hengimg_two" :src="item.thumb" alt="">
                <div class="hengright_two">
                    <span class="hengname_two">{{item.name}}</span>
                    <span class="hengname_twos">{{item.name}}</span>
                    <div class="hengzhanweifu"></div>
                    <div class="hengdetails">{{item.details}}</div>
                </div>
            </div>
        </div>
        <!-- 竖三栏 -->
        <div v-if="Commswitch == 3">
            <div class="Commshusan" v-for="(item,key) in list" :key="key" v-if="key<shul">
                    <img class="shusanimg" :src="item.thumb" alt="">
                    <div class="Commsanlan">
                        <span class="Commshuname">{{item.name}}</span>
                        <span class="Commshuname_two">{{item.details}}</span>
                    </div>
                    <div class="Commdetails_bot">{{item.details}}</div>
                </div>
                <div class="clear"></div>
            </div>
        </div>
      <div class="right">
        <div class="Sdianzhao"><i></i><span>商品设置{{index}}</span></div>
        <div class="Commcaozuo" v-if="Comtype">
        <span class="Span" style="margin-top:0px">模块配置：</span>
            <div class="SwLunbo" style="width:140px">
                <span class="YesNoLb" style="marginRight:6px">是否显示</span>
                <div class="YesNo">
                    <span class="Yes" :class="ComynMK ? 'pitch' : '' "  @click="ComYesMK()">是</span>
                    <span class="No" :class="!ComynMK ? 'pitch' : '' "  @click="ComNoMK()">否</span>
                </div>
            </div>
        </div>
        <div class="Sshangcheng">
            <span class="shangchengname">新增商品栏：</span>
            <el-button type="primary" @click="add()" size="mini">添加商品栏</el-button>
        </div>
        <div class="Sshangcheng">
            <span class="shangchengname">标头配置：</span>
            <el-radio-group v-model="radio5" size="mini" @change="CommHead">
                <el-radio-button label="标题"></el-radio-button>
                <el-radio-button label="TAB栏"></el-radio-button>
            </el-radio-group>
        </div>
        <div class="Sshangcheng" v-if="head">
            <span class="shangchengname">商城名称：</span>
            <el-input class="shurukuang" placeholder="最多可输入6字" :maxlength="6" @blur="biaot" clearable></el-input>
        </div>
        <div class="Sshangcheng">
            <span class="shangchengname">商品样式：</span>
            <el-radio-group v-model="radio6" size="mini" @change="Theradiobutton">
                <el-radio-button label="小图"></el-radio-button>
                <el-radio-button label="大图"></el-radio-button>
                <el-radio-button label="整图"></el-radio-button>
            </el-radio-group>
        </div>
        <div class="Sshangcheng">
            <span class="shangchengname">商品显示数量：</span>
            <el-input-number v-model="shul" :step="1" :min="0" :max="10" size="mini" @change="Digitalchange" label="修改商品显示数量"></el-input-number>
        </div>
      </div>
  </div>
</template>

<script>
    import {https,Goodss,Www1,BackEnd} from '../assets/BaseApi'


    const cityOptions = ['热卖','最新','推荐','更新'];
    const aiya = [1,2,3,4];

    export default {
    name: 'Commoditybar',
    props: {
        index: Number,
        items: Array,
        color: String,
        Comtype: Boolean
    },
    data () {
        return {
            biaoti:"商品介绍",
            list:[
                {name:'商品名称',
                price:"88.88",
                market:"5234",
                boll:'true',
                details:'此处是商品详情',
                thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
                {name:'商品名称',details:'此处是商品详情',price:"88.88",market:"5234",thumb:'http://www1.xiaoniren.cn/upload/attachment/5/130/201805/15253158643349.jpg'},
            ],
            Commfenlei:[
                {name:'分类一',id:1},
                {name:'分类二',id:2},
                {name:'分类三',id:3},
                {name:'分类四',id:4}
            ],
            radioNames: [],
            checkedNames: [],
            // TAB栏和title切换
            head:false,
            // Tab切换
            Tabindex:0,
            // 删除事件需要的参数
            aid: 1,
            // checkAll: false,
            checkedCities: [1,2,3,4],
            cities: cityOptions,
            // isIndeterminate: true,
            ComShowHide:true,
            // 帮助显示隐藏
            Helpcenter: false,
            // Comtype:true,
            type:'',
            ComynMK:true,
            // UI单选
            radio5: 'TAB栏',
            radio6: '小图',
            style: [],
            // 切换样式
            Commswitch:1,
            // 网址
            shul:4,
            bol: false,
            https:'https://www.xiaoniren.cn'
        }
    },
    mounted: function () {
        var getUrlStr =  function(name) {
    　　var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    　　var r = window.location.search.substr(1).match(reg);
        　　if(r != null) return unescape(r[2]);
        　　return null;
        }
        // console.log('再次获取URL--id')
        var id = getUrlStr ("id")
        var type = getUrlStr("type")

        this.colum(id,type)
        // this.ajax()
    },
    methods:{
        colum (id,type) {
            var _this = this
            if (type == 'front') {
                this.type = Www1
            }else if (type == 'back'){
                this.type = BackEnd+id
            }
            this.$ajax.get(this.type)
            .then(function (res) {
                // 取值
                if (res.data.ext.extAppid == 0) {
                    var data = res.data.ext.ext
                    if (data.flag) {
                    for (var k in data.flag ) {
                        if (k == _this.index) {
                            _this.Commswitch = data.flag[k].style || 1
                            _this.biaoti = data.flag[k].tag || _this.biaoti
                            _this.shul = data.flag[k].num || _this.shul
                            _this.ComynMK = data.flag_display
                            _this.radio6 = data.flag[k].classify || _this.radio6
                            _this.head = data.flag[k].type
                            _this.radio5 = data.flag[k].classtype || _this.radio5
                            // 把字符串转化成整数数组
                            _this.checkedCities = data.flag[k].flag || _this.checkedCities
                            var dataStrArr = _this.checkedCities.split(",")
                            var dataIntArr = []
                            var dataIntArr =  dataStrArr.map(function(data){  
                                return + data;
                            })
                            _this.checkedCities = dataIntArr
                        }
                    }
                    } 
                }else {
                    var data = res.data.ext.ext
                    if (data.flag) {
                    for (var k in data.flag ) {
                        if (k == _this.index) {
                            _this.Commswitch = data.flag[k].style || 1
                            _this.biaoti = data.flag[k].tag || _this.biaoti
                            _this.shul = data.flag[k].num || _this.shul
                            _this.ComShowHide = data.flag_display
                            _this.ComynMK = data.flag_display
                            _this.radio6 = data.flag[k].classify || _this.radio6
                            _this.head = data.flag[k].type
                            _this.radio5 = data.flag[k].classtype || _this.radio5
                            console.log(data.flag[k].flag)
                            // 把字符串转化成整数数组
                            _this.checkedCities = data.flag[k].flag || _this.checkedCities
                            var dataStrArr = _this.checkedCities.split(",")
                            var dataIntArr = []
                            var dataIntArr =  dataStrArr.map(function(data){  
                                return +data;
                            })
                            _this.checkedCities = dataIntArr
                        }
                    }
                    }
                }     
                // _this.ajax()
            })
            .catch(function (err) {
                console.log(err)
                console.log('失败了')
            });
        },
        ComYesMK () {
            this.ComynMK = true
            var Show = true
            this.$emit('ComShowHide',Show)
        },
        ComNoMK () {
            this.ComynMK = false
            var Hide = false
            this.$emit('ComShowHide',Hide)
        },
        dataDetails() {
            this.bol = true
        },
        hiddenDetail() {
            this.bol = false
        },
        // 标头显示隐藏
        CommHead (value) {
            if (value == '标题') {
                this.head = true
                this.$emit('CommHead',true,value,this.index)
            }else {
                this.head = false
                this.$emit('CommHead',false,value,this.index)
            }
        },
        // 帮助显示隐藏
        HelpcenterShow() {
            this.Helpcenter = true
        },
        HelpcenterHide() {
            this.Helpcenter = false
        },
        //删除 
        open2() {
            this.$confirm('删除当前商品组件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
                if (this.index > 0) {
                    this.items.splice(this.index,1,2);
                    console.log(this.items)
                }else{
                    return this.$message({
                        message: '至少保留一条商品栏 , 如想隐藏请点击模块配置“否”',
                        type: 'warning'
                    });
                }
            this.$emit('FlagItems', this.items,this.index)
            this.$message({
                type: 'success',
                message: '删除成功!',
            });
            }).catch(() => {
            this.$message({
                type: 'info',
                message: '已取消删除'
            });          
            });
        },
        // 商品数量改变
        Digitalchange (value) {
            this.$emit('digital',value, this.index)
        },
        TabComm (id) {
            this.Tabindex = id
        },
        // 标题
        biaot(err) {
            this.biaoti = err.target.value
            this.$emit('biaoti',this.biaoti,this.index)
        },
        handle(value) {
            this.checkedCities = value
            // this.ajax()
            this.$emit('duoxuan',this.checkedCities,this.index)
        },
        // 添加
        add() {
            let normal = {
                tag: '商品栏',
                style: 1,
                flag: '1,2,3,4',
                num: 4,
                classify: '小图',
                type:false,
                classtype:'TAB栏'
            }
            this.items.push({
                'component': "Commoditybar"
            })
            console.log(this.items)
            this.$emit('AddPush',normal)
        },
        // 单选
        Theradiobutton(value) {
            if ( value == '小图' ) {
                this.Commswitch = 1
                this.checkedCities = [1,2,3,4]
                this.$emit('fenlei',1,this.index,value)
                this.$emit('duoxuan',this.checkedCities,this.index) 
            }else if ( value == '整图' ) {
                this.Commswitch = 3
                this.checkedCities = [1,2,3,4]
                this.$emit('fenlei',3,this.index,value)   
                this.$emit('duoxuan',this.checkedCities,this.index)
            }else if ( value == '大图' ) {
                this.Commswitch = 2
                this.checkedCities = [1,2,3,4]
                this.$emit('fenlei',2,this.index,value)   
                this.$emit('duoxuan',this.checkedCities,this.index)
            }
        },
        ajax () {
            var _this = this
            _this.list = []
            this.$ajax.get(Goodss, {
                params: {
                merchant_id : 130,
                page : 1,
                per_page : 100
                }
            })
            .then(function (res) {
                var data = res.data.data.items
                for (var key in data) {
                    for (var i in data[key].flag) {
                        for (var j in _this.checkedCities) {
                            if (data[key].flag[i] == _this.checkedCities[j]) {  
                                _this.list.push(data[key])
                            }
                        }
                    }
                }
                // 数组去重
                var hash = {}; 
                _this.list = _this.list.reduce(function(item, next) { 
                hash[next.name] ? '' : hash[next.name] = true && item.push(next); 
                return item 
                }, [])
                // console.log(style)
                // 判断商品栏横竖
                // for (var k in _this.list) {
                //     for (var i in style) {
                //         if ( k = i) {
                //             _this.list[k]['style'] = (style[i])
                //         }
                //     }
                // }
            })
            .catch(function (err) {
                console.log(err);
            });
        }
    }  
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
    @import '../../css.less';
    .Commoditybar {
        width: 100%;
        position: relative;
        padding-bottom: 5px;
    }
    .Cbody {
        width: 100%;
        border: 1px solid #fff;
        box-sizing:border-box;
    }
    .Cbody:hover {
        cursor: move;
        border-left: 1px dashed #409EFF;
        border-right: 1px dashed #409EFF;
        border-top: 1px solid #409EFF;
        border-bottom: 1px solid #409EFF;
    }
    .clear{ clear:both} 
    #shanchu {
        position: absolute;
        top: 0px;
        right: 0px;
        padding: 0;
        cursor:pointer;
        color: #606266;
        z-index: 2000;
    }
    #shanchu:hover {
        color: #66b1ff;
    }
    .col {
        font-size: 20px;
    }
    .Commtitle {
        padding: 10px;
        padding-left: 0px;
        font-size: 18px;
        text-align: left;
    }
    .Commi {
        display: inline-block;
        width: 3px;
        height: 16px;
        background-color: #000;
    }
    .right {
        display: none;
        font-size: 14px;
        color: @Lightgrey;
        min-width: 550px;
        cursor: default;
        position: fixed;
        left: 35%;
        top: 8px;
        padding: 28px 17px 15px 17px;
        text-align: left;
        background: #f8f8f8;
        border-radius: 5px;
        border: 1px solid #d1d1d1;
        z-index: 1999;
    }
    input {
        text-indent:1em;
        color: @Lightgrey;
        border-radius: 5px;
        border:1px solid #DBDBDB;
    }
    // 组件修改值
    .ele {
        margin-left: 10px !important;
    }
    .Sshangcheng .el-checkbox-group {
        display: inline-block;
        width: 300px;
    }
    // 商品TAB栏
    .CommUl {
        width: 100%;
        box-sizing: border-box;
        display: flex;
        margin-bottom: 10px;
    }
    .Coomli {
        flex: 1;
        padding: 10px 0px;
        text-align: center;
        font-size: 14px;
        color: #999;
        list-style: none;
        border-bottom: 1px solid #ccc;
        cursor: pointer;
    }
    .Coomred {
        color: #2cb3de;
        border-bottom: 1px solid #2cb3de;
    }
    .henglie {
        width: 100%;
        box-sizing: border-box;
        margin: 0 auto;
        height: 80px;
        padding: 5px;
    }
    .hengimg {
        width: 27.3%;
        height: 100%;
        border-radius: 5px;
        float: left;
    }
    .hengright {
        width: 68%;
        height: 100%;
        float: right;
        position: relative;
    }
    .hengjiage {
        position: absolute;
        left: 0px;
        bottom: 0px;
    }
    .subscribe {
        display: inline;
        padding: 4px 23px;
        font-size: 14px;
        border-radius: 15px;
        float: right;
        color: #fff;
        background-color: #35b0fe;
    }
    .hengname {
        font-size: 13px;
        margin-top: 2px;
        width: 100%;
        color: #000;
        display: inline-block;
        text-align: left;
        overflow: hidden;
        display:-webkit-box;
        -webkit-box-orient:vertical;
        -webkit-line-clamp:2;
    }
    .yangtoufu {
        font-size: 12px;
        color: @red;
    }
    // 横二栏
    .henglie_two {
        width: 100%;
        box-sizing: border-box;
        margin: 0 auto;
        height: 130px;
        padding: 5px;
    }
    .hengimg_two {
        width: 37.3%;
        height: 100%;
        border-radius: 5px;
        float: left;
    }
    .hengright_two {
        width: 60%;
        height: 100%;
        float: right;
        position: relative;
    }
    .subscribe_two {
        display: inline;
        padding: 4px 23px;
        font-size: 14px;
        border-radius: 15px;
        float: right;
        color: #fff;
        background-color: #35b0fe;
    }
    .hengname_two {
        font-size: 15px;
        margin-top: 2px;
        width: 100%;
        color: #d96666;
        display: inline-block;
        text-align: left;
        overflow: hidden;
        display:-webkit-box;
        -webkit-box-orient:vertical;
        -webkit-line-clamp:1;
    }
    .hengname_twos {
        font-size: 15px;
        margin-top: 2px;
        width: 100%;
        color: #ccc;
        display: inline-block;
        text-align: left;
        overflow: hidden;
        display:-webkit-box;
        -webkit-box-orient:vertical;
        -webkit-line-clamp:1;
    }
    .hengzhanweifu {
        width: 100%;
        height: 30px;
        border-bottom: 1px solid #ccc;
    }
    .hengdetails {
        font-size: 13px;
        margin-top: 2px;
        width: 100%;
        color: #ccc;
        display: inline-block;
        text-align: left;
        overflow: hidden;
        display:-webkit-box;
        -webkit-box-orient:vertical;
        -webkit-line-clamp:1;
    }
    // 结束
    // 横三栏
    .Commshusan {
        padding: 2px 4px;
        position: relative;
    }
    .shusanimg {
        width: 100%;
        height: 120px;
    }
    .Commsanlan {
        position: absolute;
        top: 7px;
        left: 14px;
        border-left: 1px solid #fff;
        padding: 0px 10px;
    }
    .Commshuname {
        font-size: 16px;
        padding-bottom: 3px;
        color: #fff;
        text-align: left;
        overflow:hidden; /*超出的部分隐藏起来。*/ 
        text-overflow:ellipsis;/* 支持 IE */
        display:-webkit-box; 
        -webkit-box-orient:vertical;
        -webkit-line-clamp:1;
    }
    .Commshuname_two {
        font-size: 14px;
        color: #fff;
        text-align: left;
        overflow:hidden; /*超出的部分隐藏起来。*/ 
        text-overflow:ellipsis;/* 支持 IE */
        display:-webkit-box; 
        -webkit-box-orient:vertical;
        -webkit-line-clamp:1;
    }
    .Commdetails_bot {
        width: 60%;
        height: 40px;
        text-align: left;
        color: #fff;
        position: absolute;
        left: 14px;
        bottom: 7px;
        font-size: 13px;
        overflow:hidden; /*超出的部分隐藏起来。*/ 
        text-overflow:ellipsis;/* 支持 IE */
        display:-webkit-box; 
        -webkit-box-orient:vertical;
        -webkit-line-clamp:1;
    }
    // 竖
    .jiaf {
        font-size: 16px;
        color: @red;
    }
    .jiar {
        font-size: 10px;
        color: #ccc;
    }
    .shugoumai {
        position: absolute;
        right: 5px;
        bottom: 5px;
        padding: 2px;
        color: @white;
        background-color: @red;
        border-radius: 5px;
        font-size: 10px;
    }
    .Commcaozuo {
        width: 100%;
        height: 32px;
        line-height: 32px;
        margin-bottom: 10px;
    }
    .CommHelpcenter {
        display: inline-block;
        width: 50px;
        cursor: pointer;
        position: relative;
    }
    .Commbangzhu {
        display: inline-block;
        width: 20px;
        height: 20px;
    }
    .HelpStep {
        position: absolute;
        top: 0;
        left: 30px;
        width: 250px;
        line-height: 20px;
        padding: 0px 5px;
        background-color: #fff;
        border: 1px solid #ccc;
        border-radius: 5px;
    }
</style>