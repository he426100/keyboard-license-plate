<template>
  <div id="key-board">
    <div v-show="type >= 1" class="key-board">
      <ul class="clearfix ul_keybord">
        <li 
          @click="input(item, index)"
          v-for="(item, index) in keyList2" :key="item" 
          :class="{
            ikey:true, 
            ['ikey' + index]: true,
            ['ikeycode' + item]: true,
            li_num: index < 10,
            li_zm: index > 9,
            li_w: index > 28,
            disabled: (item == 'I' || item == 'O') || (item !=='删除' && ((type === 1 && index < 10) || type === 3))
          }"
        >
          <span>{{item}}</span>
        </li>
      </ul>
      <ul class="clearfix ul_keybord">
        <li
          @click="input2(item, index)"
          v-for="(item, index) in keyList3" :key="item"
          :class="{
            disabled: car_type == 2 || cValue.length < 6 || (type === 1 && index < 6) || type === 3
          }">
          <span>{{item}}</span>
        </li>
      </ul>
    </div>
    <div v-show="type === 0" class="key-board">
      <ul class="clearfix ul_pro">
        <li 
          @click="chooseProvince(item);"
          v-for="item in keyList1" :key="item">
          <span>{{item}}</span>
        </li>
        <!-- <li class="li_close" onclick="closePro();">
          <span>关闭</span>
        </li> -->
        <!-- <li class="li_clean" onclick="cleanPro();">
          <span>清空</span>
        </li> -->
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'key-board',
  props: {
    value: {
      type: Array,
      default: () => []
    },
    car_type: 0
  },
  data () {
    return {
      cValue: this.value,
      keyList1: ['京', '沪', '浙', '苏', '粤', '鲁', '晋', '冀', '豫', '川', '渝', '辽', '吉', '黑', '皖', '鄂', '津', '贵', '云', '桂', '琼', '青', '新', '藏', '蒙', '宁', '甘', '陕', '闽', '赣', '湘'],
      keyList2: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P', 'A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'Z', 'X', 'C', 'V', 'B', 'N', 'M', '删除'],
      keyList3: ['港', '澳', '学', '警', '领', '使']
    }
  },
  computed: {
    type () {
      switch (this.value.length) {
        case 0:
          return 0
        case 1:
          return 1
        case 7:
          return this.car_type != 2 ? 3 : 2
        case 8:
          return this.car_type == 2 ? 3 : 2
        default:
          return 2
      }
    }
  },
  methods: {
    chooseProvince (val) {
      this.cValue.splice(0, 1, val)
    },
    input (val, index) {
      if (val === '删除') {
        this.cValue.pop()
      } else {
        if ((this.type === 1 && index < 10) || this.type === 3) return
        this.cValue.push(val)
      }
    },
    input2 (val, index) {
      if ((this.type === 1 && index < 6) || this.type === 3) return
      this.cValue.push(val)
    }
  },
  watch: {
    value () {
      this.cValue = this.value
    },
    cValue () {
      this.$emit('input', this.cValue)
    },
    car_type () {
      if (this.car_type != 2 && this.cValue.length === 8) {
        this.cValue.pop()
      }
    }
  }
}
</script>

<style lang="less">
#key-board {
  .clearfix:after {
    content: ".";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
  ul {
    background-color: #ced3d9;
    text-align: center;
    padding: 4px 2px;
    font-size: 16px;
    li {
      list-style: none;
      :active {
        background-color: #b9c2cf;
        color: #ffffff;
      }

      &.disabled span {
        color: #DDDDDD;
        background-color: #fff;
      }
      
    }
    span {
      display: block;
      background-color: #fff;
      border-radius: 4px;
      box-shadow: 2px 2px 2px #888888;
      line-height: 32px;
      padding-top: 2px;
    }
  }
  .ul_pro {
    li {
      float: left;
      width: 11.11%;
      padding: 2px;
      box-sizing: border-box;
    }
  }
  .ul_keybord {
    li {
      float: left;
      width: 10%;
      padding: 2px;
      box-sizing: border-box;
      &.ikeycodeDel {
        width: 22%;
      }
    }
  }

  .ul_pro .li_close {
    float: right;
    width: 22.22%;
  }
  .ul_pro .li_clean {
    float: right;
    width: 22.22%;
  }
  .ul_keybord .li_w {
    width: 11.11%;
  }
  .ul_keybord .ikey20 {
    margin-left: 5%;
  }
  .ikeycodeOK {
    opacity: 0;
  }
  
}
</style>
