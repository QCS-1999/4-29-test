<template>
  <div>
    <button @click="disA">{{ dis }}</button>
    <br />
    <br />
    <span>城市：</span>
    <select v-model="city">
      <option v-for="(item, index) in section" :key="index" :value="item.name">
        {{ item.name }}
      </option>
    </select>
    <br />
    <br />
    <span>用户名:</span
    ><input type="text" v-model="userName" :disabled="isDis" />
    <br />
    <br />
    <span>年龄:</span
    ><input type="number" style="width: 40px" v-model="age" :disabled="isDis" />
    <br />
    <br />
    <button @click="add" :disabled="isDis">{{ post }}</button>
    <br />
    <br />

    <div>
      <span>筛选：</span>
      <input type="text" v-model="keyword" :disabled="isDis" />
      <span>年龄:</span>
      <input
        type="text"
        v-model="keyage1"
        style="width: 20px"
        :disabled="isDis"
      />~
      <input
        type="text"
        v-model="keyage2"
        style="width: 20px"
        :disabled="isDis"
      />
    </div>
    <!-- 内容部分 -->
    <table border="1" style="border-collapse: collapse; width: 800px">
      <thead>
        <tr style="background: #ccc">
          <th></th>
          <th>id</th>
          <th>姓名</th>
          <th>年龄 <van-icon name="arrow-up" @click="sort" /></th>
          <th>城市 <van-icon name="arrow-up" @click="sort1" /></th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in search"
          :key="index"
          align="center"
          :class="{ bgcolor: index % 2 }"
          style="background-color: pink"
        >
          <td>
            <input
              type="checkbox"
              :value="index"
              v-model="checkedAll"
              :disabled="isDis"
            />
          </td>
          <td>{{ item.id }}</td>
          <td>{{ item.userName }}</td>
          <td>{{ item.age }}</td>
          <td>{{ item.city }}</td>
          <td>
            <button @click="edit(item, index)" :disabled="isDis">修改</button>
            <button @click="del(index)" :disabled="isDis">删除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <button @click="delAll" :disabled="isDis">删除已选</button>
  </div>
</template>

<script>
import section from "./assets/data.json";
import { pinyin } from "pinyin-pro";
export default {
  data() {
    return {
      dis: "锁定",
      post: "提交",
      section: section.data,
      city: "",
      userName: "",
      age: "",
      testList: [],
      userId: 1,
      infoIndex: "",
      checkedAll: [],
      isDis: false,
      time: 4,
      keyword: "",
      Arr: ["北京", "广州", "上海", "成都"],
      keyage1: "",
      keyage2: "",
    };
  },
  created() {},
  methods: {
    //添加
    add() {
      let obj = {
        id: this.userId,
        userName: this.userName,
        city: this.city,
        age: this.age,
      };
      if (this.post == "提交") {
        this.testList.push(obj);
        this.userId++;
      } else {
        obj.id = this.testList[this.infoIndex].id;
        this.testList.splice(this.infoIndex, 1, obj);
        this.post == "提交";
      }
      this.city = "";
      this.userName = "";
      this.age = "";
    },
    //删除
    del(index) {
      this.testList.splice(index, 1);
    },
    //修改
    edit(test, index) {
      this.city = test.city;
      this.age = test.age;
      this.userName = test.userName;
      this.infoIndex = index;
      this.post = "确认修改";
    },
    //选中删除
    delAll() {
      this.checkedAll
        .sort((a, b) => {
          return b - a;
        })
        .forEach((item) => {
          this.testList.splice(item, 1);
        });
      this.checkedAll = [];
    },
    //锁定
    disA() {
      if (this.dis == "锁定") {
        this.isDis = true;
        this.dis = "解锁";
      } else {
        let timer = setInterval(() => {
          this.time--;
          this.dis = this.time + "秒后解锁";
          if (this.time == 0) {
            clearInterval(timer);
            this.dis = "锁定";
            this.isDis = false;
            this.time = 4;
          }
        }, 1000);
      }
    },
    //年龄排序
    sort() {
      this.testList.sort((a, b) => {
        return a.age - b.age;
      });
    },
    //城市排序
    sort1() {
      this.testList.sort((a, b) => {
        return pinyin(a.city, { pattern: "first" }) <
          pinyin(b.city, { pattern: "first" })
          ? -1
          : 1;
      });
    },
  },
  computed: {
    search() {
      if (this.keyage1 != "" && this.keyage2 != "") {
        return this.testList.filter((item) => {
          return (
            Number(this.keyage1) <= Number(item.age) &&
            Number(item.age) <= Number(this.keyage2)
          );
        });
      } else {
        return this.testList.filter((item) => {
          return (
            item.city.indexOf(this.keyword) > -1 ||
            item.userName.indexOf(this.keyword) > -1
          );
        });
      }
    },
  },
};
</script>


<style>
.bgcolor {
  background-color: lightcoral !important;
}
</style>
