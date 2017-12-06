
<template>
   <div id='tmpl'>
   <!-- 评论组件 -->
   <!--1.0 实现提交评论数据到服务器的静态结构-->
    	<div id="postcomment">
   			<h3>提交评论</h3>
			<p class="p"></p>
			<textarea placeholder="请输入您要评论的内容..." v-model="postcontent"></textarea>
			<mt-button type="primary" size="large" @click="postcomment">发表</mt-button>
		</div>
	<!-- 获取评论数据的列表 -->
		<div id="list">
			<h3>评论列表</h3>
			<p class="p"></p>
			<div v-for="(item,index) in list">
				<div class="title" >
					
					<span>第{{index+1}}楼:</span>
					<span>用户：{{item.user_name}}</span>
					<span>发表时间:{{item.add_time | datefmt('YYYY-MM-DD HH:MM:SS')}}</span>
				</div>
				<ul class="mui-table-view">
					<li class="mui-table-view-cell" v-text="item.content"></li>
				</ul>
			</div>
			
		</div>
   
   </div>
</template>,
<script>
import { Toast } from 'mint-ui';
import common from '../../kits/common.js';
export default {
  data(){
  	return {
  		postcontent:'' ,//用来自动获取用户输入的内容;
  		list:[]
  	}
  },

  props:['id'],//用来接受父组件传递过来的id值
  created(){
  	this.getcommentlist();
  },
  methods:{
  	//获取当前的评论数据;pageindex:代表的是当前获取的是哪一页的数据，默认值是1
  	getcommentlist(pageindex){
  		pageindex = pageindex || 1;
//				1.0 确定评论数据的url
				var url  = common.apidomain + '/api/getcomments/'+this.id+'?pageindex='+pageindex;
//				2.0 发出ajax请求获取数据即可
				this.$http.get(url).then(function(res){
					if(res.body.status != 0 ){
						Toast(res.body.message);
						return;
					}
//3.0 				将message数组中的数据赋值给this.list
					this.list = this.list.concat(res.body.message);
				});

  	},
  	// 评论数据的提交
  	postcomment(){
  		//0.0判断用户是否填写数据
  		if(this.postcontent.trim().length<=0){
  			Toast("您评论的内容不能为空");
  			return;
  		}
  		// 1.0 确定提交的地址 /api/postcomment/:artid
  		var url=common.apidomain+'/api/postcomment/'+this.id;
  		// 2.0 利用ajax的post请求将数据提交到指定的地址
  		this.$http.post(url,{content:this.postcontent},{emulateJSON:true}).then(function(res){
					Toast(res.body.message);
					//	3.0 将文本框中的评论内容清空
					this.postcontent = '';
				});
//				2.0.1 获取到用户在文本框中填写的评论内容，通过 content: 内容 格式提交到请求报文体中
//	3.0 将文本框中的评论内容清空

  	}
  }
}
</script>
<style scoped>
	#postcomment {
		padding: 7px;
	}
	 .p{
		height: 1px;
		width: 100%;
		border-bottom: 1px solid rgba(0,0,0,0.3);
	}
	#list{
	padding: 5px;
		}
	.title{
	padding: 5px;
	color: #6d6d72;
	font-size: 15px;
	background-color: rgba(0,0,0,0.1);
	}
</style>
 
