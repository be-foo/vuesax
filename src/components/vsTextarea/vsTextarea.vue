<template lang="html">
  <div
    :style="style"
    :class="[`vs-textarea-${color}`, {'textarea-danger': counter ? (value && value.length > counter) : false, 'focusx': isFocus}]"
    class="vs-component vs-con-textarea">

    <h4 v-if="label">
      {{ label }}
    </h4>

    <textarea
      :value="value"
      v-bind="$attrs"
      class="vs-textarea"
      v-on="listeners"
      :style="textareaStyle">
    </textarea>

    <div
      v-if="counter"
      class="count vs-textarea--count">
      {{ value ? value.length : 0 }} / {{ counter }}
    </div>

  </div>
</template>

<script>
import _color from '../../utils/color.js'
export default {
  name: "VsTextarea",
  inheritAttrs:false,
  props:{
    value:{},
    label:{
      default:null,
      type: String
    },
    color:{
      default:'primary',
      type:String
    },
    counter:{
      default: null,
      type: [Number, String]
    },
    counterDanger:{
      default: false,
      type: Boolean
    },
    height:{
      default:null,
      type: String
    },
    width:{
      default:null,
      type: String
    },
    resize:{
      default: true,
      type: Boolean
    }
  },
  data:()=>({
    isFocus: false
  }),
  computed:{
    style() {
      const style = {}
      style.border = `1px solid ${this.isFocus?_color.getColor(this.color,1):'rgba(0, 0, 0,.08)'}`
      //style.height = this.height
      style.width = this.width

      return style
    },
    textareaStyle() {
      const style = {};
      style.height = this.height;
      if (!this.resize) {
        style.resize = 'none';
      }
      return style;
    },
    listeners() {
      return {
        ...this.$listeners,
        input:(evt) => {
          this.$emit('input', evt.target.value)
        },
        focus:() => {
          this.focus()
        },
        blur:() => {
          this.blur()
        }
      }
    }
  },
  watch:{
    value() {
      if(this.value && this.value.length > this.counter) {
        this.$emit('update:counterDanger', true)
      } else {
        this.$emit('update:counterDanger', false)
      }
    }
  },
  methods:{
    focus() {
      this.isFocus = true
      this.$emit('focus')
    },
    blur() {
      this.isFocus = false
      this.$emit('blur')
    }
  }
}

</script>
