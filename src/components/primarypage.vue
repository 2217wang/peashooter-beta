<template>
    <div>
      <myhead></myhead>
      <!--主体-->
      <div>
        <div  class="m-container m-padded-tb-big animated fadeIn ">
          <div class="ui container">
            <div class="ui stackable grid">

              <!--左边广场列表-->
              <div class="eleven wide column" >
                <!--header-->

                  <div class="ui middle aligned one column grid">
                    <div class="column">
                      <div class="ui raised segment">

                        <a class="ui teal header">豌豆射手主页&emsp;&emsp;&emsp;&emsp;</a>
                        <a class="right aligned ">
                          共 <h2 class="ui red header m-inline-block m-text-thin" > {{posttotal}} </h2> 条发布内容&emsp;&emsp;&emsp;&emsp;
                        </a>
                        <el-button type="primary" @click="mypost">我要发布<i class="el-icon-upload el-icon--right"></i></el-button>
                      </div>
                    </div>

                  </div>


                <!--content-->

                <div class="ui attached  segment m-margin-top " style="border-radius: 15px" v-for="(item,index) in posts">
                  <div class="ui padded vertical segment" >
                    <div class="ui  mobile reversed stackable grid" >
                      <div class="eleven wide column left aligned " >
                        <h3 class="ui header left aligned" ><a   target="_blank" class="m-black" >{{item.title}}</a></h3>
                        <div class="ui red horizontal label ">资源地址：</div><div class="m-text" >{{item.url}}</div>
                        <div class="ui orange horizontal label ">评分：<span class="m-text" >{{item.score}}</span></div>
                        <div class="ui yellow horizontal label ">标签：<span class="m-text" >{{item.tags}}</span></div>
                        <el-card shadow="hover" style="background-color: #dddddd" class="m-margin-top-small">
                          {{item.content}}
                        </el-card>
                        <div class="ui grid m-margin-top-small">
                          <div class="column">
                            <div class="ui mini horizontal link list">
                              <div class="item">
                                <img :src="circleUrl"   alt="" class="ui avatar image">
                                <div class="content"><a  class="header"  >{{item.username}}</a></div>
                              </div>
                              <div class="item">
                                <i class="calendar icon"></i><span >{{formatDate(new Date(item.create_time*1000))}}</span>
                              </div>
                              <div class="item">
                                <a @click="dianzan(item.id,item)"><img  class="middle aligned" style="width: 18px" :src="likesimage"><span >{{item.likes}}</span></a>
                              </div>
                              <div class="item">
                                <a @click="getcommentbypostid(item.id,index)"><img  class="middle aligned" style="width: 18px" src="../../static/images/comment.png"></a>
                              </div>
                            </div>
                          </div>

                        </div>
                      </div>

                      <div class="five wide column middle aligned" >
                        <a href="#"  target="_blank">
                          <img :src="item.imageurl"   alt="" class="ui rounded image">
                        </a>
                      </div>

                      <div class="ui bottom attached segment" v-if="item.isShowComment" >
                        <div  class="ui bottom attached " >
                          <!--留言区域列表-->
                          <div id="comment-container"  class="ui teal segment">
                            <div >
                              <div class="ui threaded comments" style="max-width: 100%;">
                                <h3 class="ui dividing header">评论</h3>
                                <p v-if="isempty">评论区为空</p>
                                <div class="comment" v-for="(item,index) in thispagecomments" :key="item.id">
                                  <a class="avatar">
                                    <img :src="circleUrl" >
                                  </a>
                                  <div class="content" style="text-align: left">
                                    <a class="author" >
                                      <span >{{item.username}}</span>
                                    </a>
                                    <div class="metadata">
                                      <span class="date">{{formatDate(new Date(item.create_time*1000))}}</span>
                                    </div>
                                    <div class="text" >
                                      {{item.content}}
                                    </div>
                                    <div class="actions" >
                                      <a class="reply" data-commentid="1" data-commentnickname="Matt"  @click="reply(item.user_id)" >回复</a>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                          </div>
                          <br/>
                          <div id="comment-form" class="ui form m-margin-tb-big ">
                            <input type="hidden" name="blog.id" >
                            <input type="hidden" name="parentComment.id" value="-1">
                            <div class="field">
                              <textarea name="content" id="sub" placeholder="请输入评论信息..." v-model="addComment.content"></textarea>
                            </div>
                            <div class="fields">
                              <div class="field  m-margin-bottom-small m-mobile-wide"style="width: 100%">
                                <!--<button id="commentpost-btn " type="button" class="ui teal button m-mobile-wide" @onclick="submitForm"><i class="edit icon"></i>发布</button>-->
                                <el-button type="primary" @click="submitForm">发布</el-button>
                              </div>
                            </div>

                          </div>
                        </div>
                      </div>


                    </div>
                  </div>


                </div>




                <!--footer-->
                <div class="ui bottom attached segment m-margin-top" >
                  <div class="ui middle aligned">
                    <!--<div class="ui inverted buttons m-padded-tb-large">-->
                    <!--<div class="column">
                      <a href="#"    class="ui blue inverted button">上一页</a>
                    </div>
                    <div class="right aligned column">
                      <a href="#"  class="ui blue inverted button">下一页</a>
                    </div>-->
                    <!--</div>-->
                    <!--分页-->
                    <el-pagination
                      @current-change="handleCurrentChange"
                      background
                      layout="prev, pager, next"
                      :page-size="5"
                      :total="posttotal"
                      :current-page="page">
                    </el-pagination>
                  </div>
                </div>

              </div>

              <!--右边的top-->
              <div class="five wide column">

                <!--logo-->
                <div class="ui  segments ">
                  <div class="ui blue segment">
                    <img src="../../static/images/pooshooter.png"  style="width:100%"/>
                  </div>
                </div>
                <!--最新推荐-->
                <div class="ui segments m-margin-top-large">
                  <div class="ui blue segment ">
                    <i class="bookmark icon"></i>最新发布
                  </div>
                  <div class="ui segment left aligned"  v-for="(item,index) in allposts">
                    <div class="ui red horizontal label">New</div><a  target="_blank" class="m-black m-text-thin" >{{item.title}}</a>
                  </div>

                </div>
                <!--分类-->
                <div class="ui  segments m-margin-top-large">
                  <div class="ui blue segment">
                    <div class="ui two column grid">
                      <div class="column">
                        <i class="idea icon"></i>热门话题
                      </div>
                      <div class="right aligned column">
                        <a @click="gototopic"  target="_blank">更多 <i class="angle double right icon"></i></a>
                      </div>
                    </div>
                  </div>
                  <div class="ui blue segment">
                    <div class="ui fluid vertical menu" v-for="(item,index) in topics">
                      <a  target="_blank" class="item"  >
                        <span >{{item.date.title}}</span>
                        <!--<div class="ui teal basic left pointing label" >13</div>-->
                      </a>

                    </div>
                  </div>
                </div>

                <!--标签-->
                <div class="ui segments m-margin-top-large">
                  <div class="ui blue segment">
                    <div class="ui two column grid">
                      <div class="column">
                        <i class="tags icon"></i>标签
                      </div>
                      <div class="right aligned column">
                        <a @click="gototag" >更多 <i class="angle double right icon"></i></a>
                      </div>
                    </div>
                  </div>
                  <div class="ui blue segment" >
                    <a href="#"  target="_blank" class="ui teal tag label m-margin-tb-tiny" v-for="(item,index) in tags">
                      <span >{{item.name}}</span>
                    </a>

                  </div>
                </div>



                <!--二维码-->
                <!--
                <h4 class="ui horizontal divider header m-margin-top-large">扫码关注我</h4>
                <div class="ui centered card" style="width: 11em">
                  <img src="../static/images/wechat.jpg" th:src="@{/images/wechat.jpg}" alt="" class="ui rounded image" >
                </div>
                -->
              </div>

            </div>
          </div>

        </div>
      </div>
      <myfoot></myfoot>
    </div>
</template>

<script>
    import myhead from "./myhead";
    import myfoot from "./myfoot";

    export default {
      inject:['reload'],
      data() {
        return {
          isRouterAlive :false,
          likesimage:'../../static/images/like.png',
          allposts:[],
          //page第几页
          page:1,
          posttotal:1,
          topics:[],
          tags:[],
          isempty:false,
          circleUrl: "https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png",
          howmanycomments:'',
          isShowComment:false,
          posts:[],
          thispagecomments:[],
          postInfo:{
            id:'',
            page:1
            //暂时设置1
          },
          addComment:{},
        }
      },
      methods:{
        gototopic(){
          this.$router.push("/topic")
        },
        gototag(){
          //空空如也
        },
        getcommentusername(){
          //得到用户名
          const _this=this
          for(let c in _this.thispagecomments){
            const cc=c
            console.log('拿到用户名了吗')
            console.log(_this.thispagecomments[cc].user_id)
            //user_id换用户名
            const ddd={id:_this.thispagecomments[cc].user_id}
            _this.$axios({
              method: 'post',
              url: '/api/user/searchbyid',
              data:_this.$qs.stringify(ddd),
              headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
              }
            }).then(function (res) {
              const r=res.data
              _this.thispagecomments[cc].username=r.data.username
              console.log("得到评论区的用户名成功")
            }).catch(function (res) {
              console.log("得到评论区的用户名发生异常！请稍后重试...")
            })
          }
          console.log('拿到添加用户名的评论区')
          console.log(_this.thispagecomments)
        },
        //处理页码改变后的posts数据
        async handleCurrentChange(current){
          /*this.posts=[]*/
          this.page=current
          console.log('当前页码为')
          console.log(this.page)
          const _this=this
          const xx={page:this.page}
          await this.$axios({
            method: 'post',
            url: '/api/posts/get',
            data:this.$qs.stringify(xx)
          }).then(function (res) {
            console.log("得到的发布表总信息")
            console.log(res)
            const __this =_this
            const r=res.data
            _this.posts=r.data
            console.log("得到的发布表总信息")
            console.log(_this.posts)
            for(let c in _this.posts){
              console.log("c" + "是啥")
              console.log(_this.posts[c])
              _this.posts[c].isShowComment=false
              _this.posts[c].imageurl="http://129.204.247.165/"+_this.posts[c].route
              let prepostid=_this.posts[c].id
              const likes={id:prepostid}
              _this.$axios({
                method: 'post',
                url: '/api/posts/getlike',
                data: _this.$qs.stringify(likes),
              }).then(function (res) {
                _this.posts[c].likes=res.data.data
                console.log('获取某个post点赞数成功')
                console.log(c.likes)
              })
                .catch(function (res) {
                  console.log('获取某个post点赞数失败')
                })
            }
            _this.updatedata()
          }).catch(function (res) {
            console.log(res)
            console.log("添加isShowComment获取发布信息发生异常！请稍后重试...")
          })
          console.log('该页下的数据')
          console.log(_this.posts)
          /*this.$router.go(0);*/
          /*this.reload()*/
        },
        async dianzan(postid,thisitem){
          window.sessionStorage.setItem('primarypagenum',this.page)
          const _this=this
          thisitem.likesimage='../../static/images/liked.png'
          const zan={
            post_id:postid,
            user_id:window.sessionStorage.getItem('user_id')
          }
          console.log('点赞表参数')
          await this.$axios({
            method: 'post',
            url: '/api/likes',
            data:this.$qs.stringify(zan)
          }).then(function (res) {
            _this.$message.success("点赞成功！")
            console.log(res)
            console.log('点赞成功')
            console.log('报错了吗')
            /*_this.getList()*/
          }).catch(function (res) {
            console.log(res)
            console.log("点赞异常！请稍后重试...")
          })
          thisitem.likes=parseInt(thisitem.likes)+1
          console.log('前端点赞数+1后的结果')
          console.log(thisitem.likes)
          thisitem.likesimage='../../static/images/liked.png'
          this.reload()
          /*setTimeout(this.reload(), 1000);*/
          /*this.$router.go(0);*/
        },
        async submitForm(){
          this.addComment.user_id=window.sessionStorage.getItem('user_id')
          this.addComment.post_id=this.postInfo.id
          const _this=this
          console.log('要加入的评论数据')
          console.log(this.addComment)
          await this.$axios({
            method: 'post',
            url: '/api/comments',
            data:this.$qs.stringify(this.addComment)
          }).then(function (res) {
            _this.$message.success("添加评论成功！")
            console.log(res)
            console.log('报错了吗')
            _this.reload()
            console.log('报错了吗')
            /*_this.getList()*/
          }).catch(function (res) {
            console.log(res)
            console.log("添加评论异常！请稍后重试...")
          })
          this.$router.go(0);
          /*_this.reload()*/
        },
        updatedata(){
          const _this=this
          for(let c in _this.posts){
            _this.posts[c].isShowComment=false
            _this.posts[c].imageurl="http://129.204.247.165/"+_this.posts[c].route
            _this.posts[c].likesimage='../../static/images/like.png'
            let prepostid=_this.posts[c].id
            const likes={id:prepostid}
            _this.$axios({
              method: 'post',
              url: '/api/posts/getlike',
              data: _this.$qs.stringify(likes),
            }).then(function (res) {
              _this.posts[c].likes=res.data.data
              console.log('获取某个post点赞数成功')
              console.log(c.likes)
            })
              .catch(function (res) {
                console.log('获取某个post点赞数失败')
              })
          }
        },
        async hanshu(){
          const _this =this
          //获取post
          const x={page:window.sessionStorage.getItem('primarypagenum')}
          await this.$axios({
            method: 'post',
            url: '/api/posts/get',
            data:this.$qs.stringify(x),
          }).then(function (res) {
            const __this =_this
            const r=res.data
            _this.posts=r.data
            console.log("得到的发布表总信息")
            console.log(_this.posts)

            /*//posts增加属性username
            for(let c in _this.posts){
              const cc=c
              console.log('拿到用户名了吗')
              console.log(_this.posts[c].user_id)
              //user_id换用户名
              const ddd={id:_this.posts[c].user_id}
              _this.$axios({
                method: 'post',
                url: '/api/user/searchbyid',
                data:_this.$qs.stringify(ddd),
                headers: {
                  'Content-Type': 'application/x-www-form-urlencoded'
                }
              }).then(function (res) {
                const r=res.data
                _this.posts[cc].username=r.data.username
                console.log("得到的用户名成功")
              }).catch(function (res) {
                console.log("得到的用户名发生异常！请稍后重试...")
              })
            }*/
            console.log('拿到添加用户名的post')
            console.log(_this.posts)

          }).catch(function (res) {
            console.log(res)
            console.log("添加isShowComment获取发布信息发生异常！请稍后重试...")
          })
          await this.updatedata()
        },
        mypost(){
          this.$router.push("/addpost")
        },
        async getcommentbypostid(param,index){
          this.thispagecomments=[]
          window.sessionStorage.setItem('primarypagenum',this.page)
          const _this=this
          this.postInfo.id=param
          console.log('对应postid为')
          console.log(this.postInfo.id)
          console.log('this是哪个')
          console.log(this)
          /*this.isShowComment=!this.isShowComment*/
          /*isShowComment=!isShowComment*/
          this.posts[index].isShowComment=!this.posts[index].isShowComment
          console.log('isShowComment改变了嘛')
          console.log(this.posts)
          //获取comment
          await this.$axios({
            method: 'post',
            url: '/api/posts/getcomment',
            data:this.$qs.stringify(this.postInfo),
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            }
          }).then(function (res) {
            const r=res.data
            if(res.data.success)
              _this.thispagecomments=r.data
            console.log("得到的某发布页评论信息")
            console.log(res)
            console.log(_this.thispagecomments)

          }).catch(function (res) {
            console.log(res)
            console.log("获取某发布页评论信息发生异常！请稍后重试...")
          })
          if(_this.thispagecomments.length===0){
            _this.isempty=true
          }else{
            _this.isempty=false
          }
          /*await _this.getcommentusername()*/
          //重新渲染页面
          /*this.reload()*/
          /*//posts增加属性username
          for(let c in _this.posts){
            const cc=c
            console.log('拿到用户名了吗')
            console.log(_this.posts[c].user_id)
            //user_id换用户名
            const ddd={id:_this.posts[c].user_id}
            _this.$axios({
              method: 'post',
              url: '/api/user/searchbyid',
              data:_this.$qs.stringify(ddd),
              headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
              }
            }).then(function (res) {
              const r=res.data
              _this.posts[cc].username=r.data.username
              console.log("得到的用户名成功")
            }).catch(function (res) {
              console.log("得到的用户名发生异常！请稍后重试...")
            })
          }
          console.log('拿到添加用户名的post')
          console.log(_this.posts)*/
          /*setTimeout(this.$forceUpdate(),5000)*/
          await this.$forceUpdate();
          /*this.$router.go(0);*/
        },
        formatDate(date) {
          var year=date.getFullYear();
          var month=date.getMonth()+1;
          var day=date.getDate();
          var hour=date.getHours();
          var minute=date.getMinutes();
          var second=date.getSeconds();
          //  return year+"-"+month+"-"+date+" "+hour+":"+minute+":"+second;
          return year + '-' + (String(month).length > 1 ? month : '0' + month) + '-' +
            (String(day).length > 1 ? day : '0' + day) + ' ' + (String(hour).length > 1 ? hour : '0' + hour) + ':' + (String(minute).length > 1 ? minute : '0' + minute)
            + ':' + (String(second).length > 1 ? second : '0' + second)
        },
      },
      components:{
        myhead:myhead,
        myfoot:myfoot
      },
      name: "primarypage",
      async created() {
        /*window.sessionStorage.setItem('primarypagenum',1)*/
        this.page=window.sessionStorage.getItem('primarypagenum')
        const _this =this
        //全部post
        await this.$axios({
          method: 'post',
          url: '/api/posts/getall',
        }).then(function (res) {
          console.log("获取全部post成功")
          console.log(res)
          _this.allposts=res.data.data


        }).catch(function (res) {
          console.log(res)
          console.log("获取全部post失败")
        })
        //增加comment
        /*const d={
          content:'好棒啊啊啊',
          user_id:1,
          post_id:3
        }
        console.log(d)
        this.$axios({
          method: 'post',
          url: '/api/comments',
          data:this.$qs.stringify(d),
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          },
        }).then(function (res) {
          console.log(res)
          console.log("添加评论成功")

        }).catch(function (res) {
          console.log("添加评论失败")
        })*/
        //获取全部post个数
        this.$axios({
          method: 'post',
          url: '/api/posts/getcount',
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          },
        }).then(function (res) {
          console.log(res)
          _this.posttotal= parseInt(res.data.data)
          console.log("查找全部post条数成功")

        }).catch(function (res) {
          console.log("查找全部post条数失败")
        })
        await this.hanshu()

        console.log('进过这里了吗')
        console.log('加了赞之后的post')
        console.log(_this.posts)
        console.log("添加isShowComment后的发布表总信息")
        console.log(_this.posts)

        console.log('难道这里面是空的吗')
        console.log(_this.posts)
        /*for(let m in _this.posts) {
          console.log('进过这里了吗2')
          let user_id = m.user_id
          const uu = {id: user_id}
          _this.$axios({
            method: 'post',
            url: '/api/user/searchbyid',
            data: _this.$qs.stringify(uu),
          }).then(function (r) {
            console.log(r)
            //将用户id保存至sessionstorage
            console.log('获取某个用户信息成功')
          }).catch(function (res) {
            console.log('获取某个用户信息失败')
          })
        }*/

        //通过id改用户名
        /*console.log('难道这里面是空的吗')
        console.log(_this.posts)
        for(let m in _this.posts){
          console.log('进过这里了吗2')
          let user_id=m.user_id
          const uu = {id:user_id}
          _this.$axios({
            method: 'post',
            url: '/api/user/searchbyid',
            data: this.$qs.stringify(uu),
          }).then(function (r) {
            console.log(r)
            //将用户id保存至sessionstorage
            console.log('获取某个用户信息成功')
          }).catch(function (res) {
            console.log('获取某个用户信息失败')
          })
        }*/

        //获取话题
        this.$axios({
          method: 'post',
          url: '/api/topics/gethot',
        }).then(function (res) {
          const r=res.data
          _this.topics=r.data
          console.log('获取的话题榜')
          console.log( _this.topics)
        }).catch(function (res) {
          console.log("获取话题发生异常！请稍后重试...")
          console.log(res)
        })

        //获取标签
        this.$axios({
          method: 'get',
          url: '/api/tags',
        }).then(function (res) {
          const r=res.data
          _this.tags=r.data
          console.log('获取的标签榜')
          console.log( _this.topics)
        }).catch(function (res) {
          console.log("获取标签发生异常！请稍后重试...")
          console.log(res)
        })


      }
    }
</script>

<style scoped>

</style>
