<template>
  <div class="wave">
    <div class="wave-wrap">
      <div class="wave-border">
        <div class="wave-cont">
          <div class="wave-cont-inner">
            <div class="wave wave1" ref="wave1"></div>
            <div class="wave wave2" ref="wave2"></div>
            <div class="percent">
              <p>{{ percent }}%</p>
              <p>{{ name }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'b-wave',
    data () {
      return {
        percent: 0,
        percentTimer: null,
        positionTimer: null
      }
    },
    props: ['propValue', 'name'],
    watch: {
      propValue (val) {
        this.animate(val)
      }
    },
    methods: {
      percentAnimate (num) {
        let speed = 800 / num
        clearInterval(this.percentTimer)
        this.percentTimer = setInterval(() => {
          if (num > this.percent) {
            this.percent ++
          } else {
            this.percent = num
            clearInterval(this.percentTimer)
          }
        }, speed)
      },
      positionAnimate (num) {
        clearInterval(this.positionTimer)
        this.positionTimer = setInterval(() => {
          if (num > this.percent) {
            this.$refs.wave1.style.cssText = 'position: absolute; top:' + (120 - (this.percent * 1.4)) + 'px'
            this.$refs.wave2.style.cssText = 'position: absolute; top:' + (120 - (this.percent * 1.4)) + 'px'
          } else {
            clearInterval(this.positionTimer)
          }
        }, 5)
      },
      animate (num) {
        this.percent = 0
        this.percentAnimate(num)
        this.positionAnimate(num)
      }
    },
    mounted () {
      this.animate(this.propValue)
    }
  }
</script>
<style lang='scss' scoped>

  .wave{
    *{
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    @keyframes rotateTaiji {
      0%{
        transform:rotate(0deg);
      }
      100%{
        transform:rotate(-360deg);
      }
    };
    width: 230px;
    height: 230px;
    margin: 0 auto;
    overflow: hidden;
    /*background-color: red;*/
    &-wrap{
      width: 230px;
      height: 230px;
      border-radius: 115px;
      position: relative;
      .wave-border{
        width: 230px;
        height: 230px;
        overflow: hidden !important;
        border-radius: 115px;
        border: 7px  solid #13adff;
        padding: 8px;
        .wave-cont{
          width: 200px;
          height: 200px;
          border-radius: 100px;
          overflow: hidden !important;
          .wave-cont-inner{
            width: 200px;
            height: 200px;
            border-radius: 100px;
            overflow: hidden !important;
            position: relative;
            transform: rotate(0deg);
            .wave{
              position: absolute;
              top: 200px;
              left: 0;
              /*background-color: red;*/
              width: 400%;
              height: 150%;
              background-image: url("./wave.png");
              background-size: 800px 100%;
            }
            .wave1{
              transform: translateY(0px);
              margin-top:-5px;
              opacity: .5;
              animation:sploosh 4s linear both infinite;
              -webkit-animation:sploosh 4s linear both infinite;
            }
            .wave2 {
              opacity: .8;
              animation:sploosh 5s linear both infinite;
              -webkit-animation:sploosh 5s linear both infinite;
            }
            .percent{
              position: absolute;
              top: 55px;
              text-align: center;
              left: calc(50% - 100px);
              width: 200px;
              height: 100px;
              color: white;
              p{
                color: #eeeeee;
              }
              p:nth-child(1){
                font-size: 48px;
                height: 67px;
              }
              p:nth-child(2){
                height: 24px;
                font-size: 21px;
              }
            }
          }
        }
      }
    }
    @-webkit-keyframes sploosh{
      0% {background-position: 200px bottom;}
      100% {background-position: 600px bottom;}
    }
    @keyframes sploosh{
      0% {background-position: 200px bottom;}
      100% {background-position: 600px bottom;}
    }
  }

</style>
