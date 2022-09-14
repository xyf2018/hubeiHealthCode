<template>
  <div class="up">
    <!--div class="name_wrap">
      <div class="name">{{ name }}</div>
      <div class="idNo">{{ idNo }}</div>
    </div-->
    <div class="time_wrap">
      <div class="time">
        <div class="clock">{{ dateNow }} <a v-html="timeNow"></a></div>
      </div>
      <span class="hesuan_time">{{ hesuan_time }}</span>
      <!--div class="caiyang_wrap">
        <span>核酸</span> <span class="caiyang">已采样</span>
        <span class="caiyang_time">{{ caiyang_time }}</span>
      </div-->
    </div>
  </div>
  <!--div class="down"></div-->
</template>

<script>
export default {
  name: "IndexComp",
  props: {},
  data() {
    return {
      hesuan_time: "",
      caiyang_time: "",
      dateNow: "",
      timeNow: "",
      name: "",
      idNo: "",
      nameList: ["凯", "军", "凤", "伟", "倩", "南", "然", "雨", "哲", "路"],
      xing: "",
      clock: null
    };
  },
  methods: {
    rand(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
    padLeftZero: (str) => {
      return ("00" + str).substr(str.length);
    },
    formatDate: function (date, fmt) {
      if (typeof date == "object") {
        date = new Date(date);
      } else if (typeof date == "string" && date.indexOf("-") != -1) {
        date = new Date(date.replace(/-/g, "/"));
      } else {
        date = new Date(date);
      }
      if (/(y+)/.test(fmt)) {
        fmt = fmt.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
      }
      let o = {
        "M+": date.getMonth() + 1,
        "d+": date.getDate(),
        "h+": date.getHours(),
        "m+": date.getMinutes(),
        "s+": date.getSeconds(),
      };
      for (let k in o) {
        if (new RegExp(`(${k})`).test(fmt)) {
          let str = o[k] + "";
          fmt = fmt.replace(RegExp.$1, RegExp.$1.length === 1 ? str : this.padLeftZero(str));
        }
      }
      return fmt;
    },
    check(a) {
      if (a < 10) return "0" + a;
      else return a;
    },
    getToday() {
      var datas = new Date();
      var on1 = "-";
      var on2 = ":";
      var onY = datas.getFullYear(); //年
      var onM = datas.getMonth() + 1; //月
      var onD = datas.getDate(); //日
      var onS = datas.getHours(); //时
      var onF = datas.getMinutes(); //分
      var onN = datas.getSeconds(); //秒

      if (onM >= 1 && onM <= 9) {
        //月
        onM = "0" + onM;
      }
      if (onD >= 1 && onD <= 9) {
        //日
        onD = "0" + onD;
      }
      if (onS >= 0 && onS <= 9) {
        //时
        onS = "0" + onS;
      }
      if (onF >= 0 && onF <= 9) {
        //分
        onF = "0" + onF;
      }
      if (onN >= 0 && onN <= 9) {
        //秒
        onN = "0" + onN;
      }
      return {
        date: onY + on1 + onM + on1 + onD,
        time: onS + on2 + onF + on2 + `<span> ` + onN + `</span>`, //onS + on2 + onF + on2 + onN
      };
    },
  },
/*  created() {
    if (confirm("是否自动生成姓名，身份证号")) {
      this.name = "**" + this.nameList[Math.floor(Math.random() * 10)];
      this.idNo = "****** ******** " + this.rand(1000, 9999);
    } else {
      let name = prompt("输入你的姓名", "");
      this.idNo = "****** ******** " + prompt("输入你的身份证号后4位", "");
      for (let i = 0; i < name.length - 1; i++) {
        this.xing += "*";
      }
      this.name = this.xing + name.charAt(name.length - 1);
    }
  }, */
  mounted() {
    this.hesuan_time = this.formatDate(new Date(), "yyyy-MM-dd hh:mm");
    this.caiyang_time = this.formatDate(new Date(), "yyyy-MM-dd");

     this.clock = setInterval(() => {
      this.dateNow = this.getToday().date;
      this.timeNow = this.getToday().time;
    });
    //document.querySelector('.disclaimer').style.display = 'none';我用的免费空间有个广告 屏蔽一下
  },
  beforeUnmount() {
    clearInterval(this.clock);
  }
};
</script>
<style>
.up {
  background: url("../assets/bg.jpg");
  background-size: 100% 100%;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
  /*max-height: 644px;*/
}

/*.down {
  background: url("../assets/bg2.jpg");
  background-size: 100% 100%;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
  max-height: 266px;
} */

.time {
  position: absolute;
  top: 132px;
  width: 100%;
  text-align: center;
  font-size: 20px;
  font-weight: bold;
}

.clock span {
  color: #4d94f3;
  font-size: 24px;
  font-weight: bold;
  font-family: helvetica Neue !important;
  margin-left: -6px;
}

/*.caiyang_wrap {
  position: absolute;
  left: 50%;

  top: 400px;
  transform: translateX(-50%);

  display: table;
  min-width: 200px;
  color: #65ba6e;
  text-align: center;
  align-items: center;
} */

.hesuan_time {
  color: rgba(255, 255, 255, 1);
  font-size: 12px;
  position: absolute;
  top: 543px;
  left: 60px;
}

/*.caiyang_time {
  color: #65ba6e;
  font-size: 16px;
}

.caiyang {
  font-size: 22px;
  padding: 0 8px;
  font-weight: 600;
}

.name_wrap {
  position: absolute;
  display: flex;
  color: rgba(255, 255, 255, 0.7);
  top: 90px;
  left: 25px;
}

.idNo {
  margin-left: 60px;
} */
</style>
