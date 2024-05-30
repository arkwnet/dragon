<template>
  <div>
    <div class="header">
      <div class="wrapper">
        <h1>好きなジェネレータ発表ドラゴン</h1>
      </div>
    </div>
    <div class="main">
      <div class="wrapper">
        <div class="message" v-if="isMessage">
          {{ message }}
        </div>
        <div class="photo"><img :src="imageUrl" /></div>
        <canvas class="canvas" ref="canvas" width="1600" height="900"></canvas>
        <div class="control">
          <p>
            <textarea
              class="textarea font-36px"
              v-model="script"
              placeholder="こ↑こ↓は最大2行"
            ></textarea>
          </p>
          <p>
            <input
              type="text"
              class="text font-24px"
              v-model="subtitle"
              placeholder="好きな歌詞発表ドラゴン"
            />
          </p>
          <div class="button" @click="load">生成</div>
        </div>
        <p class="center">
          右クリックメニュー「名前を付けて画像を保存」またはロングタップで保存できます。
        </p>
      </div>
    </div>
    <br />
    <div class="wrapper">
      <Adsense :data-ad-client="adClient" :data-ad-slot="adSlot"> </Adsense>
    </div>
    <div class="footer">
      <div class="wrapper">
        <p>
          画像はンバヂ氏の
          <a href="https://commons.nicovideo.jp/works/nc355601" target="blank">nc355601</a>,
          <a href="https://commons.nicovideo.jp/works/nc355602" target="blank">nc355602</a>
          を合成
        </p>
        <br />
        <p>Version {{ version }}</p>
        <p>
          Copyright (c) 2024 <a href="https://arkw.net/">Sora Arakawa</a><br />Licensed under the
          MIT License
        </p>
        <p><a href="https://github.com/arkwnet/dragon">GitHub Repository</a></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      status: false,
      image: new Image(),
      imageUrl: '',
      imageWidth: 1600,
      imageHeight: 900,
      canvas: null,
      context: null,
      adClient: import.meta.env.VITE_AD_CLIENT,
      adSlot: import.meta.env.VITE_AD_SLOT,
      version: import.meta.env.PACKAGE_VERSION,
      script: 'ﾎﾞﾀﾝ一発で\n作れるやつ',
      subtitle: '好きなジェネレータ発表ドラゴン',
      isMessage: false,
      message: ''
    }
  },
  mounted() {
    this.$gtag.pageview('/')
    this.message = import.meta.env.VITE_MESSAGE
    if (this.message != '' && this.message != undefined) {
      this.isMessage = true
    }
    this.imageUrl = import.meta.env.BASE_URL + 'img/loading.png'
    this.canvas = this.$refs.canvas
    this.context = this.canvas.getContext('2d')
    const vm = this
    document.fonts.ready.then(function () {
      setTimeout(function () {
        vm.draw(import.meta.env.BASE_URL + 'img/default.png')
        vm.status = true
      }, 1000)
    })
  },
  methods: {
    load() {
      if (this.status == true) {
        this.draw(import.meta.env.BASE_URL + 'img/default.png')
      }
    },
    draw(src) {
      let vm = this
      vm.image.src = src
      vm.image.onload = function () {
        vm.context.drawImage(vm.image, 0, 0, vm.imageWidth, vm.imageHeight)
        vm.context.font = '90px "GenJyuuGothic-Medium", sans-serif'
        vm.context.fillStyle = 'black'
        const array = vm.script.split('\n')
        if (array.length == 1) {
          vm.context.fillText(
            vm.script,
            175 + (500 - vm.context.measureText(vm.script).width) / 2,
            358
          )
        } else if (array.length == 2) {
          vm.context.fillText(
            array[0],
            175 + (500 - vm.context.measureText(array[0]).width) / 2,
            295
          )
          vm.context.fillText(
            array[1],
            175 + (500 - vm.context.measureText(array[1]).width) / 2,
            422
          )
        }
        vm.context.font = '77px "GenJyuuGothic-Medium", sans-serif'
        vm.context.fillText(
          vm.subtitle,
          (vm.imageWidth - vm.context.measureText(vm.subtitle).width) / 2,
          844
        )
        vm.imageUrl = vm.canvas.toDataURL()
      }
    }
  }
}
</script>
