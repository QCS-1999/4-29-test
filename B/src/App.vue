<template>
  <div class="qcs_home">
    <div class="qcs_left">
      <div class="qcs_login">
        <p>
          用户名：<input type="text" v-model="qcs_user" v-if="isUeserShow" />
          <span v-else>{{ qcs_user }}</span>
          <span v-show="qcs_show" style="color: red"
            >用户名长度只能是6~18位</span
          >
        </p>
        <br />
        <button @click="qcs_login">{{ login }}</button>
      </div>
    </div>
    <div class="qcs_right">
      <div class="motai" v-show="isMoShow"></div>
      <div
        v-for="(item, index) in qcs_userWen"
        :key="index"
        v-show="qcs_index == index"
      >
        <h1>{{ item.title }}</h1>
        <ul>
          <li v-for="(itm, idx) in item.options" :key="idx">
            <input type="radio" :value="idx" v-model="qcs_changeIndex" />{{
              itm
            }}
          </li>
        </ul>
        <button @click="qcs_next(index)">提交答案</button>
        <p class="qcs_bottom">
          共有{{ qcs_index + 1 }}/4道题,答对：{{ qcs_right }}答错：{{
            qcs_lass
          }}
        </p>
      </div>
      <div v-show="qcs_index == 4">
        已经没题了<button @click="qcs_clear">重新答题</button>
        <p class="qcs_bottom">
          共有{{ qcs_index + 1 }}/4道题,答对：{{ qcs_right }}答错：{{
            qcs_lass
          }}
        </p>
      </div>
    </div>
  </div>
</template>


<script>
import userWen from "./assets/data.json";
export default {
  data() {
    return {
      login: "登录",
      isMoShow: true,
      isUeserShow: true,
      qcs_user: "",
      qcs_show: false,
      qcs_userWen: userWen.data,
      qcs_index: 0,
      qcs_right: 0,
      qcs_lass: 0,
      qcs_changeIndex: "",
    };
  },
  created() {
    console.log(this.qcs_userWen);
  },
  methods: {
    //登录
    qcs_login() {
      if (this.login == "登录") {
        if (this.qcs_user.length >= 6 && this.qcs_user.length <= 18) {
          this.qcs_show = false;
          this.isMoShow = false;
          this.login = "退出";
          this.isUeserShow = false;
        } else {
          this.qcs_show = true;
          this.isMoShow = true;
          this.isUeserShow = true;
        }
      } else {
        (this.isUeserShow = true), (this.isMoShow = true);
        this.qcs_index = 0;
        this.qcs_right = 0;
        this.qcs_lass = 0;
        this.qcs_changeIndex = "";
        this.qcs_user="";
      }
    },
    //提交答案
    qcs_change(index) {
      setTimeout(() => {
        this.qcs_changeIndex = index + 1;
      }, 5000);
    },
    // 提交答案
    qcs_next(index) {
      if (this.qcs_changeIndex == this.qcs_userWen[index].right) {
        this.qcs_index++;
        this.qcs_right++;
        this.qcs_changeIndex = "";
      } else {
        this.qcs_changeIndex = "";
        this.qcs_index++;
        this.qcs_lass++;
      }
    },
    //重新答题
    qcs_clear() {
      this.qcs_index = 0;
      this.qcs_right = 0;
      this.qcs_lass = 0;
      this.qcs_changeIndex = "";
    },
  },
};
</script>



<style>
* {
  margin: 0;
  padding: 0;
}
.qcs_home {
  width: 100%;
  height: 100%;
  display: flex;
}
.qcs_left {
  width: 50%;
  height: 500px;
  border: 1px solid #000;
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login p span {
  color: red;
}
.qcs_right {
  flex: 1;
  border: 1px solid #000;
  position: relative;
}
.qcs_right li {
  list-style: none;
}
.qcs_bottom {
  width: 100%;
  height: 80px;
  position: absolute;
  bottom: 0;
}
.motai {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.9);
  position: absolute;
  top: 0;
  left: 0;
}
</style>
