<template>
  <div class="wrapper" v-show="isShow">
    <div class="mask"></div>
    <transition name="scale-fade" @after-leave="afterLeave">
      <div class="container" v-show="isShow">
        <div class="card" :class="rotateClass">
          <div class="face front-face" @click="toggleRotate">
            <img class="face" src="./img/card_front_face.png">
          </div>
          <div class="face back-face" @click="toggleRotate">
            <img class="card-img" src="./img/card_back_face.png">
          </div>
        </div>
        <div class="close-wrapper">
          <close @click="closeLayer"></close>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import { Close } from '@/components/common'
  export default {
    name: 'rotate-card-layer',
    components: {
      Close
    },
    props: {
      isShow: Boolean
    },
    data () {
      return {
        rotateClass: ''
      }
    },
    methods: {
      toggleRotate () {
        this.rotateClass = this.rotateClass ? '' : 'rotate180'
      },
      closeLayer () {
        this.$emit('update:isShow', false)
      },
      afterLeave () {
        this.$emit('afterLeave', null)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .scale-fade-enter-active, .scale-fade-leave-active {
    transition: all 2s ease;
  }
  .scale-fade-enter, .scale-fade-leave-to {
    opacity: 0;
    transform: scale(0);
  }
  .wrapper {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background: rgba(0, 0, 0, .8);
    .mask {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
    }
    .container {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
  }
  .card {
    position: relative;
    width: 5.2rem;
    height: 7.05rem;

    .face {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      transition: transform 2s ease;
      backface-visibility: hidden;
      .card-img {
        width: 100%;
        height: 100%;
      }
      &.back-face {
        transform: rotateY(180deg);
      }
    }

    &.rotate180 {
      .face {
        &.front-face {
          transform: rotateY(-180deg);
        }
        &.back-face {
          transform: rotateY(0);
        }
      }
    }
  }
  .close-wrapper {
    display: flex;
    justify-content: center;
    margin: .4rem 0 0;
  }
</style>
