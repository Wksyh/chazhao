# chazhao


---------------------searchlist.wxml

<view class="root">
<input class="input" placeholder="请输入要搜索的词"
bindinput="getKey"></input>
<view bindtap="goSearch">搜索</view>
</view>
<view wx:if="{{list&&list.length>0}}">
搜索结果如下
<view wx:for="{{list}}" wx:key='index'>
<view class="item">
<view>姓名：{{item.name}}</view>
<view>手机号：{{item.phone}}</view>
</view>
</view>
</view>
<view wx:if="{{list&&list.length==0}}">
搜索内容为空
</view>


------------------------searchlist.wxss

/* pages/searchlist/searchlist.wxss */
.root{
    display: flex;
    padding: 15rpx;
}
.input{
    flex:1;
    border:1px solid gray;
    border-radius:30rpx;
    margin-right:30rpx;
    padding-left:25rpx;
}
.item{
    color:gray;
    margin:20rpx;
    border-bottom:1px solid gainsboro;
}


--------------searchlist.json
{
  "usingComponents": {}
}


---------------searchlist.js
let db = wx.cloud.database()

Page({
    data:{
        key:null
    },
    getKey(e){
       // console.log(e.detail.value)
        this.setData({
            key:e.detail.value
        })
    } ,
    goSearch(){
    console.log(this.data.key)
    let key = this.data.key
     if(this.data.key){
         console.log('可以执行搜索')
         db.collection('addresslist')
       .where({
           name:db.RegExp({//正则匹配
               regexp:key,//要搜索的词
               options:'i',//不区分大小写
           })
       })
       .get()
       .then(res=>{
           console.log('请求到的数据',res)
           this.setData({
               list:res.data
           })
       })
     }else{
          wx.showToast({
             icon:'error',
             title:'请输入内容',
         })
     }
    },
    /**
     * 页面的初始数据
     */
    // data: {

    // },

    // /**
    //  * 生命周期函数--监听页面加载
    //  */
    // onLoad: function (options) {

    // },

    // /**
    //  * 生命周期函数--监听页面初次渲染完成
    //  */
    // onReady: function () {

    // },

    // /**
    //  * 生命周期函数--监听页面显示
    //  */
    // onShow: function () {

    // },

    // /**
    //  * 生命周期函数--监听页面隐藏
    //  */
    // onHide: function () {

    // },

    // /**
    //  * 生命周期函数--监听页面卸载
    //  */
    // onUnload: function () {

    // },

    // /**
    //  * 页面相关事件处理函数--监听用户下拉动作
    //  */
    // onPullDownRefresh: function () {

    // },

    // /**
    //  * 页面上拉触底事件的处理函数
    //  */
    // onReachBottom: function () {

    // },

    // /**
    //  * 用户点击右上角分享
    //  */
    // onShareAppMessage: function () {

    // }
})









