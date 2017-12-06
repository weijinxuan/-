 
 <template>
   <div id='tmpl'>
   <!-- 1.实现新闻的详情 -->
   		<div class="title">
   			<h4>{{info.title}}</h4>
   			<p>{{info.add_time | datefmt('YYYY-MM-DD')}} {{info.click}}次浏览</p>
   		</div>
   		<div id="content" v-html="info.content">
   			
   		</div>
      <!-- 子组件的评论的实现 -->
      <comment :id="id"></comment>

   </div>
 </template>
 <script>
 import common from '../../kits/common.js';
 // 1.导入子组件
 import comment from '../subcom/comment.vue'
 export default {
      components:{
          comment //注册评论组件
        },
        data(){
            return {
                id: 0 , // 传入的id值
                info: {
                   /*"id": 13,
                   "title": "1季度多家房企利润跌幅超50% 去库存促销战打响",
                   "click": 1,
                   "add_time": "2015-04-16T03:50:28.000Z",
                   "content":"sdfsdf"*/
               }
                
 
            }
          },
        created(){
        	//获取url中的id值给data里面里面的id;
        	this.id=this.$route.params.id;
        	//请求服务器获取详情数据;
        	this.getinfo();

        },
        methods:{
        	getinfo(){
        		var url=common.apidomain+"/api/getnew/"+this.id;
        		//ajax请求数据;
        		this.$http.get(url).then(function(res){
        			var res=res.body;
        			if(res.status!=0){
        				alert(res.status);
        				return;
        			}
        			this.info=res.message[0];
        		})
        	}
        }
  
   
 }
 </script>
 <style  scoped>
 .title h4{
		color: #0094ff;
	}
	.title p{
		color:rgba(0,0,0,0.5);
	}

	.title,#content{
		padding: 5px;
	}

 </style>
 
