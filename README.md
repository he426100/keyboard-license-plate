# keyboard-license-plate
vue车牌号模拟键盘

```
<div class="car-number">
  <div class="flexbox vux-1px" @click.stop="showKeyBoard = true" :gutter="0">
    <div class="item vux-1px-l" v-for="(v, i) in carNumberArr" :key="i">
      <div class="flex">{{carNumber[i] || ''}}</div>
    </div>
  </div>
</div>

...

<key-board v-show="showKeyBoard" v-model="carNumber" :car_type="type"></key-board>

...

import KeyBoard from '@/components/common/KeyBoard.vue'
export default {
  components: { KeyBoard },
  data () {
    return {
        carNumberArr: ['', '', '', '', '', '', ''],
		    carNumber: [],
        showKeyBoard: true,
        type: '0',
    }
  },
  watch: {
    carNumber () {
      if ((this.type != 2 && this.carNumber.length === 7) || this.carNumber.length === 8) {
        this.showKeyBoard = false
      }
    },
    type () {
      if (this.type == 2) {
        if (this.carNumberArr.length == 7) {
          this.carNumberArr.push('')
          this.showKeyBoard = true
        }
      } else if (this.carNumberArr.length == 8) {
        this.carNumberArr.pop()
        this.showKeyBoard = false
      }
    }
  }
}
```
