---
title: vue-video-player通过自定义按钮组件实现全屏切换效果
date: 2018-08-28 10:31:11
tags:
    - Vue
  
---
最近公司的产品上线，一些高级功能在基础版本中不对用户开发，通过视频的形式展示。
<!-- more -->
产品开发用的是 vue, 经同事介绍使用了 [vue-video-player](https://surmon-china.github.io/vue-video-player/) 视频播放插件，通过 [demo](https://github.com/surmon-china/vue-video-player/blob/master/examples/01-video.vue)案例很快实现了视频播放效果

![](https://user-gold-cdn.xitu.io/2018/8/28/1657f54f7392d57b?w=644&h=386&f=jpeg&s=28359)

```html
<video-player
    class="vjs-custom-skin"
    ref="videoPlayer1"
    :options="playerOptions"
    :playsinline="true"
    :events="events"
    @play="onPlayerPlay($event)"
    @pause="onPlayerPause($event)"
    @ended="onPlayerEnded($event)"
    @loadeddata="onPlayerLoadeddata($event)"
    @waiting="onPlayerWaiting($event)"
    @playing="onPlayerPlaying($event)"
    @timeupdate="onPlayerTimeupdate($event)"
    @canplay="onPlayerCanplay($event)"
    @canplaythrough="onPlayerCanplaythrough($event)"
    @ready="playerReadied"
    @statechanged="playerStateChanged($event)">
</video-player>
```
老板看了之后说: '不需要全屏切换,不需要让用户看的那么仔细',

通过配置项 `controlBar: {fullscreenToggle: false},`关闭全屏切换后，由于视频标清、展示区域大小 483px X 303px,根本看不清视频里内容，老板又说："实现全屏不铺满整个屏幕，而是通过固定大小的弹出层展示"

首先想到的是： **开启全屏切换，监听全屏切换的事件，在事件中强制退出全屏，显示固定大小的弹出层**

```
<video-player
    ...
    :events="events"
    @fullscreenchange="onPlayerFullScreenchange($event)"
    ...
>
</video-player>

// 需要在 events 中指定全屏切换事件，不然无法监听
data () {
    return {
        videoDialogVisible: false, // 控制弹出层
        events: ['fullscreenchange']
    }
},
methods: {
    ...
    onPlayerFullScreenchange (player) {
        player.exitFullscreen()  //强制退出全屏，恢复正常大小
        this.videoDialogVisible = true
    }
    ...
}
```
这种办法，虽然能实现，但在强制退出全屏那一下，整个页面会跳一下，碰到网速慢，或电脑卡的情况，效果更差...

没办法，继续想办法，在该插件 GitHub 库中，有网友提过相关 [issues](https://github.com/videojs/video.js/issues/3473), 通过 **注册一个自定义按钮组件，添加到播放器的 controlBar中,替换默认的全屏切换按钮** 

```
import * as videojs from 'video.js'

export default {
    ...
    methods: {
        ...
        createMyButton () {
            let Button = videojs.getComponent('Button')
            let myButton = videojs.extend(Button, {
                constructor: function (player, options) {
                    Button.apply(this, arguments)
                    this.addClass('fullscreen-enter')
                },
                handleClick: () => {
                    // 绑定点击事件
                },
                buildCSSClass: function () {
                  return 'vjs-icon-custombutton vjs-control vjs-button'
                }
            })
            
            //注册
            videojs.registerComponent('myButton', myButton)
            
            this.$nextTick(() => {
                // 添加到controlBar中
                this.$refs.videoPlayer1.player.getChild('controlBar').addChild('myButton')
            })
        }
        ...
    }
}

// 在 style 中指定自定义按钮样式
<style>
.video-js{
      .vjs-control-bar{
        .vjs-icon-custombutton {
          font-family: VideoJS;
          font-weight: normal;
          font-style: normal; }
          .vjs-icon-custombutton:before {
            content: "\f108";
            font-size: 1.8em;
            line-height: 1.67;
          }
      }
    }
}
</style>
```

自定义的按钮图标用的还是默认的 [全屏图标](http://videojs.github.io/font/)，接着得实现按钮的点击事件

页面和弹出层中分别是俩个 播放器实例 videoPlayer1， videoPlayer2，需要考虑到的是：当自定义切换事件触发后，俩个播放器的同步( **videoPlayer1播放了10s, 全屏切换后，videoPlayer2得从 10s 继续播放，而不是从头播放** )

```
onCustombuttonClick () {
    let _time = this.$refs.videoPlayer1.player.currentTime() //已播放时长
    this.$refs.videoPlayer2.player.currentTime(_time)
    this.$refs.videoPlayer2.player.play()
}
```

同理：关闭弹出层后， 也得把 videoPlayer2 的播放时长 赋值给 videoPlayer1，此处是通过 监听弹出层显示、隐藏等事件来实现


![](https://user-gold-cdn.xitu.io/2018/8/28/1657f9a3263db44e?w=849&h=438&f=jpeg&s=32928)