<template>
	<view class="body" >
		<view style="z-index:999;">
           <view class="uni-form-item uni-column">
     		<view class="title">周边检索</view>
     		<input class="uni-input" confirm-type="search" @blur="onKeyInput" style="border:1px solid #c2c0c0;" placeholder="请输入" />
     		</view>
              <view class="uni-form-item uni-column">
        	  <view class="title">模糊检索</view>
        		<input class="uni-input" v-model="Input" @input="bindKeyInput" confirm-type="search" style="border:1px solid #c2c0c0;" placeholder="请输入" />
				<view class="sug_info" v-if="Open">
				  <text v-for="item in dataAll" :key="item.uid" @click="handleclick(item)">{{item.name+ '\n'}}</text>
				</view>
        		</view>
				</view>
           
			 <view class="map_container"> 
			   <map class="map" id="map" :longitude="longitude" :latitude="latitude" scale="14" show-location="true" :markers="markers" @markertap="makertap"></map> 
			 </view> 
			 <view class="place_info" v-if="markerClick">
			   <text>{{placeData.address }}</text> 
			 </view>
		</view>

</template>
<script>
  var bmap = require('../../libs/bmap-wx.js');
  var wxMarkerData = [];
  var BMap = new bmap.BMapWX({
      ak: 'TfCiYsMBKdZOEFh9rGZp0IPeWRGWq8IA'
  })
	export default {
		data() {
			return {
				markers:[],
				latitude: '34.79502', 
				longitude: '117.2632', 
				placeData:{},
				Peripheral:'',//周边检索
				WXMarkerData:[],
				map:{},
				markerClick:false,
				sugData:'',
				dataAll:[],
				Open:false,
				Input:'',
        address:'',
        name:'',
			}
		},
		mounted() {
		},
		onLoad() {
		},
		methods: {
			handleclick(event){
				this.Open =false
				this.latitude=event.latitude
				this.longitude=event.longitude
				this.Input =event.name
				BMap.regeocoding({
            location: event.latitude+ ',' +event.longitude,
				    success: this.Regeocodingsuccess,
				    iconPath: '../../static/img/marker_red.png',
				    iconTapPath: '../../static/img/marker_red.png',
            width:25,
            height:25,
            name:this.Input
				});
			},
			Regeocodingsuccess(data){
        const wxMarkerData = data.wxMarkerData
        this.WXMarkerData =data.wxMarkerData
        this.markers=wxMarkerData
        this.latitude =wxMarkerData[0].latitude
        this.longitude =wxMarkerData[0].longitude
        this.address=data.originalData.result.formatted_address
        this.name=this.Input
			},
			bindKeyInput: function(e) {
			    var that = this;
			    if (e.detail.value === '') {
			        that.dataAll=[]
			        return;
			    }
			    BMap.suggestion({
			        query: e.detail.value,
			        region: '枣庄',
			        city_limit: true,
			        success: that.bindKeysuccess,
			    })
          this.markerClick=false
			},
		bindKeysuccess(data){
			 var sugData = '';
			 this.dataAll=[]
			 for(var i = 0; i < data.result.length; i++) {
			     sugData = sugData + data.result[i].name + '\n';
			 	this.dataAll.push({
			 		name:data.result[i].name,
			 		latitude:data.result[i].location.lat,
			 		longitude:data.result[i].location.lng,
			 		uid:data.result[i].uid,
          address:data.result[i].address
			 	})
			 }
			 this.sugData =sugData 
			 this.Open=true
			},
        makertap: function(e) {
          var that = this;
          var id = e.markerId;
          that.markerClick = true
          that.showSearchInfo(that.WXMarkerData, id);
          that.changeMarkerColor(that.WXMarkerData, id);
      },
      showSearchInfo: function(data, i) {
          this.placeData={
            address: '地址：' + data[i].address + '\n',
          }
      },
      changeMarkerColor: function(data, id) {
          var that = this;
          var markersTemp = [];
          for (var i = 0; i < data.length; i++) {
              if (i === id) {
                  data[i].iconPath = "../../static/img/marker_yellow.png";
              } else {
                  data[i].iconPath = "../../static/img/marker_red.png";
              }
              markersTemp[i] = data[i];
          }
          this.markers=markersTemp
      },
      onKeyInput(event){
        this.Peripheral =event.detail.value
        if (event.detail.value === '') {
            this.Peripheral=''
            return;
        }
        this.markerClick=false
       BMap.search({
           "query": this.Peripheral,
           // fail: fail,
           success: this.success,
           iconPath: '../../static/img/marker_red.png',
           iconTapPath: '../../static/img/marker_red.png',
           width:25,
           height:25,
           page_size:20,
		             
       });                  
      },
      success(data){
        this.WXMarkerData = data.wxMarkerData;
        this.markers=this.WXMarkerData
        this.latitude =this.WXMarkerData[0].latitude
        this.longitude =this.WXMarkerData[0].longitude
      }
		}
	}
	
</script>



<style scoped>
	.body{
		width: 100%;
		display: flex;
		min-height: 100vh;
		flex-direction: column;
		overflow: hidden;
	}
	.map_container{ 
	    height: 200px; 
	    width: 100%; 
	} 
	  
	.map { 
	   height: 100vh; 
	    width: 100%;
        z-index:10;
	} 
  .place_info {
      padding: 0 5px;
      background-color: #fff;
      width:100%;
      height:100rpx;
      color:#000;
      z-index: 999;
      position: absolute;
      bottom: 0;
      text-align: center;
      line-height: 100rpx;
  }
</style>

