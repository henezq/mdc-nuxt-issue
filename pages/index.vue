<template>
  <section class="container">
    <div>

      <div class="mdc-checkbox">
        <input type="checkbox"
               id="basic-checkbox"
               class="mdc-checkbox__native-control"/>
          <div class="mdc-checkbox__background">
              <svg class="mdc-checkbox__checkmark"
                   viewBox="0 0 24 24">
                <path class="mdc-checkbox__checkmark__path"
                      fill="none"
                      stroke="white"
                      d="M1.73,12.91 8.1,19.28 22.79,4.59"/>
              </svg>
              <div class="mdc-checkbox__mixedmark"></div>
          </div>
          </div>

      <h1 class="title">
        NUXT
      </h1>
      <h2 class="subtitle">
        Universal Vue.js Application
      </h2>
      <div class="links">
        <a href="https://nuxtjs.org/" target="_blank" class="button--green">Documentation</a>
        <a href="https://github.com/nuxt/nuxt.js" target="_blank" class="button--grey">GitHub</a>
      </div>
    </div>
  </section>
</template>

<script>
import { MDCCheckboxFoundation } from '@material/checkbox'

const { ANIM_END_EVENT_NAME } = MDCCheckboxFoundation.strings
export default {
  props: ['id', 'labelId', 'value'],
  data () {
    return {
      classes: {},
      changeHandlers: [],
      foundation: null
    }
  },
  mounted () {
    let vm = this
    this.foundation = new MDCCheckboxFoundation({
      addClass (className) {
        vm.$set(vm.classes, className, true)
      },
      removeClass (className) {
        vm.$delete(vm.classes, className)
      },
      registerChangeHandler (handler) {
        vm.changeHandlers.push(handler)
      },
      deregisterChangeHandler (handler) {
        let index = vm.changeHandlers.indexOf(handler)
        if (index >= 0) {
          vm.changeHandlers.splice(index, 1)
        }
      },
      registerAnimationEndHandler (handler) {
        vm.$refs.root.addEventListener(ANIM_END_EVENT_NAME, handler)
      },
      deregisterAnimationEndHandler (handler) {
        vm.$refs.root.removeEventListener(ANIM_END_EVENT_NAME, handler)
      },
      getNativeControl () {
        return vm.$refs.native
      },
      isAttachedToDOM () {
        return Boolean(vm.$el)
      }
    })
    this.foundation.init()
  },
  beforeDestroy () {
    this.foundation.destroy()
  },
  methods: {
    fireEvent (event) {
      this.changeHandlers.forEach((h) => h(event))
      this.$emit('input', event.target.checked)
    }
  },
  computed: {
    labelId () {
      return this.id + '-label'
    }
  }
}
</script>

<style lang="scss">
@import '~@material/checkbox/mdc-checkbox';

.container
{
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.title
{
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
.subtitle
{
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
.links
{
  padding-top: 15px;
}
</style>
