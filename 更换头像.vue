<template>
  <div class="homePage-information">
    <!--导航-->
    <div class="nav-bar">
      <div class="nav-b">
        <div class="nav-b">
          <van-nav-bar
            title="设置"
            left-text="返回"
            left-arrow
            @click-left="$router.push({path:'/myPersonage'})"
          />
        </div>
        <!--        <van-nav-bar title="个人信息" left-text="返回" left-arrow @click-left="$router.push({path:'/myPersonage'})">-->
        <!--          <template #left>-->
        <!--            <div>-->
        <!--              <van-icon name="arrow-left" size="18"/>-->
        <!--              <span>返回</span>-->
        <!--            </div>-->
        <!--          </template>-->
        <!--        </van-nav-bar>-->
      </div>
    </div>

    <div class="head-portrai">
      <div class="head-t">头像</div>
      <div class="img">
        <img :src="headPortrait" @click.stop="uploadHeadImg"/>
        <van-icon name="arrow"/>
      </div>
      <input type="file" accept="image/*" class="hiddenInput" @change="handleFile" style="display: none"/>
    </div>

    <div class="message">
      <div>
        <strong>用户名</strong>
        <div>
          <span>{{userinfo.userName===null?"请设置用户名":userinfo.userName}}</span>
          <van-icon name="arrow"/>
        </div>

      </div>
      <div>
        <strong>用户编号</strong>
        <div>
          <span>{{userinfo.userNo}}</span>
          <van-icon name="arrow"/>
        </div>
      </div>
      <div>
        <strong>手机号</strong>
        <div>
          <span>{{userinfo.mobileNumber}}</span>
          <van-icon name="arrow"/>
        </div>
      </div>
      <div>
        <strong>实名认证</strong>
        <div v-if="userinfo.idCard===null">
          <span>未实名认证</span>
          <i class="iconfont icon-weishimingrenzheng"></i>
        </div>
        <div v-else>
          <span>已实名认证</span>
          <i class="iconfont icon-yishimingrenzheng"></i>
        </div>
      </div>
    </div>

    <div class="btn" @click="logOut">
      <van-button>退出当前账号</van-button>
    </div>
  </div>
</template>

<script>

    import {Dialog} from 'vant';

    import {SeeMatch} from '../../models/match';

    let seeMatch = new SeeMatch();

    export default {
        name: 'homePageInformation',
        data() {
            return {
                userinfo: [],
                headPortrait: 'https://wsy-xm.github.io/imgae/img/cy.png'
            }
        },

        created() {
            this.getUserinfo()

        },

        methods: {


            uploadHeadImg() {
                this.$el.querySelector('.hiddenInput').click()
            },
            // 将头像显示
            handleFile(e) {
                console.log(e)
                let $target = e.target || e.srcElement
                let file = $target.files[0]
                var reader = new FileReader()
                reader.onload = (data) => {
                    let res = data.target || data.srcElement
                    this.headPortrait = res.result
                }
                reader.readAsDataURL(file)
            },


            logOut() {
                Dialog.confirm({
                    message: '确认退出吗？',
                })
                    .then(() => {
                        window.localStorage.clear();
                        window.sessionStorage.clear();
                        this.$router.push({path: '/'})
                    })
                    .catch(() => {
                        // on cancel
                    });
            },

            getUserinfo() {
                let userId = JSON.parse(localStorage.getItem('vuex')).loginInfo.userId;
                seeMatch.userInformationId(
                    userId
                ).then(res => {
                    this.userinfo = res.data.result
                })
            }
        }
    }
</script>

<style lang="less" scoped>
  .homePage-information {
    /*导航*/

    .nav-bar {
      background: url("../../assets/img/bj.png");
      background-size: 100% 100%;

      .nav-b {
        padding-top: 40px;
        box-sizing: border-box;

        .van-nav-bar {
          background: transparent;

          /deep/ .van-icon {
            color: white;
          }

          /deep/ span {
            color: white;
          }
        }
      }


    }

    /*头像*/

    .head-portrai {
      width: 690px;
      height: 140px;
      background: rgba(255, 255, 255, 1);
      border-radius: 5px;
      margin: 30px auto;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 30px;
      box-sizing: border-box;

      .head-t {
        font-size: 28px;
        font-family: PingFang SC;
        font-weight: 500;
        color: rgba(51, 51, 51, 1);
      }

      .img {
        img {
          width: 100px;
          height: 100px;
          background: rgba(255, 238, 237, 1);
          border: 6px solid rgba(255, 255, 255, 1);
          border-radius: 50%;
          vertical-align: middle;
        }

      }


    }

    .message {
      width: 690px;
      background: rgba(255, 255, 255, 1);
      border-radius: 5px;
      margin: 0 auto;

      div {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 15px 0 30px;
        box-sizing: border-box;
        font-size: 28px;
        font-family: PingFang SC;
        font-weight: 500;
        color: rgba(51, 51, 51, 1);
        height: 79px;

        strong {
          font-size: 28px;
          font-family: PingFang SC;
          font-weight: 500;
        }

        span {
          color: #666666;
        }
      }
    }

    .btn {
      width: 690px;
      text-align: center;
      margin-top: 42px;
      margin: 42px auto 0 auto;

      .van-button {
        width: 400px;
        height: 80px;
        background: rgba(220, 13, 13, 1);
        border-radius: 40px;
        line-height: 80px;

        .van-button__text {
          font-size: 30px;
          font-family: PingFang SC;
          font-weight: 500;
          color: rgba(255, 255, 255, 1);
        }
      }
    }

    .van-icon {
      vertical-align: middle;
      font-size: 40px;
      color: #999999;
    }

  }
</style>
