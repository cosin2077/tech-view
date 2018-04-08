<template>
  <div class="main" >
    <div class="header">
      <div class="logo">
        <a  href="#">Tech-VU</a>
      </div>
      <div class="nav">
        <mu-tabs :value="activeTab" @change="handleTabChange" class="tab">
          <mu-tab value="news" title="新闻资讯"/>
          <mu-tab value="technews" title="科技资讯"/>
          <mu-tab value="blockchain" title="区块链"/>
        </mu-tabs>
      </div>
    </div>
    <div class="content">
        <h1>{{title}}</h1>
          <div class="card-wrap">
            <div class="card-inner">
            <div class="small-card" v-for="(item,idx) in res">
              <h4 class="card-title">{{item.title}}</h4>
              <div class="card-content">{{item.summaryAuto}}</div>
              <div class="card-footer">
                <div class="card-from"><a title="点击查看来源" target="_blank" :href="item.url">{{item.siteName}}</a></div>
                <div class="card-time">时间：{{item.publishDate.slice(0,10)+','+item.publishDate.slice(11,19)}}</div>
              </div>
            </div>
            </div>
          </div>
    </div>
    <div class="footer">
      Tech-View created by <a target="_blank" href="https://github.com/conanskyforce">@conan</a>
    </div>

  </div>
</template>
<script>
export default {
  name:"Main",
  data () {
    return {
      activeTab: 'news',
      urlLists:[
        "https://api.readhub.me/news",
        "https://api.readhub.me/technews",
        "https://api.readhub.me/blockchain"
      ],
      news:null,
      technews:null,
      blockchain:null,
      res:[],
      title:'新闻资讯',
      viewHeight:document.body.clientHeight
    }
  },
  methods: {
    handleTabChange (val) {
      this.activeTab = val;
      var idx = ['news','technews','blockchain'].indexOf(val);
      this.title = ['新闻资讯','科技资讯','区块链'][idx];
      this.switchTab(val);  
    },
    checkLocal(v){
      var local = localStorage.getItem(v);
      return !!local?true:false;
    },
    switchTab(v){
      this.res = this[v];
    },
    initOne(tag){
      if(!this.checkLocal(tag)){
      var that = this;
      this.$http.get('https://api.readhub.me/'+tag)
              .then(function(result){
                that[tag] = result.data.data;
                that.res = ((tag==='news')?result.data.data:that.res);
                localStorage.setItem(tag,JSON.stringify(result.data.data));
              })
      }else{
        this[tag] = JSON.parse(localStorage[tag]);
      }
      if(tag==='news'){
        this.res = this.news;
      }
    },
    refreshOne(tag){
      var that = this;
      this.$http.get('https://api.readhub.me/'+tag)
                .then(function(result){
                  that[tag] = result.data.data;
                  that.res = (tag==that.activeTab)?result.data.data:that.res;
                  localStorage.setItem(tag,JSON.stringify(result.data.data));
                })
    },
    refreshAll(){
      var that = this;
      setInterval(function(){
        that.refreshOne('news');
        that.refreshOne('technews');
        that.refreshOne('blockchain');
      },30000)
    },
    resizeLayout(){
      var content = document.querySelector('.content');
      var cardWrap = document.querySelector('.card-wrap');
      content.style.height = document.documentElement.offsetHeight-96+'px';
      cardWrap.style.height = parseInt(content.style.height)-60+'px';
    }
  },
  mounted(){
    this.initOne('news');
    this.initOne('technews');
    this.initOne('blockchain');
    this.resizeLayout();
    this.refreshAll();
    const that = this;
    window.onresize = () =>{
      return (() =>{
        that.viewHeight = document.body.clientHeight;
      })()
    }
  },
  watch:{
    viewHeight(val){
      let that = this;
      clearTimeout(timer);
      var timer = setTimeout(function(){
        that.resizeLayout();
      },800)
    }
  }
}
</script>
<style scoped>
.main{
  background-color: rgb(236, 236, 236);
  height: 100%;
}

.header{
  background-color: #7e57c2;
}


.logo{
  font-size: 24px;
  color: white;
  display: inline-block;
  padding: 10px 20px;
}

.nav{
  display: inline-block;
  width: calc(100% - 150px);
  margin: 0 auto;
}

.tab{
  margin: 0 auto;
  width: 400px;
  background-color: rgba(0, 0, 0, 0);
}

.content{
  width: 90%;
  margin: 10px auto;
  margin-bottom: 0px;
  padding-top:10px;
  background-color: white;
  border-radius: 5px;
  min-height: 465px;
}

.content h1{
  margin-top:4px;
  margin-bottom:4px;
}

.footer{
  height: 30px;
  line-height: 30px;
  text-align: center;
}

.card-wrap{
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  min-height: 408px;
  overflow-y: scroll;
  -webkit-overflow-scrolling:touch;
}

.small-card{
  border:solid 2px whitesmoke;
  border-radius: 3px;
  max-width: 640px;
  /*min-height: 220px;*/
  margin:8px 8px;
  padding:2px;
}
.small-card h4{
  margin-top: 8px;
  margin-bottom: 8px;
}
.card-footer{
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
.logo a{
  color: white;
  border: none;
  outline: none;
}
@media (max-width: 600px){
  .tab{
    width: auto;
  }
}
.card-inner{
  min-height: calc(100% + 1px);
}
</style>