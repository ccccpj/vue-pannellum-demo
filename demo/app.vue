<template>
  <div class="app">
    <v-pannellum
      v-if="show"
      class="pannellum"
      :src="url"
      :preview="cubemapUrls.px"
      :auto-rotate="isAutoRotationOn"
      :orientation="isOrientationOn"
      :auto-load="true"
      :show-zoom="false"
      :double-click-zoom="false"
      :show-fullscreen="false"
      :compass="true"
      :hotSpots="hotSpots"
      :hfov.sync="hfov"
      :yaw.sync="yaw"
      :pitch.sync="pitch"
    >
      <p class="topic">消防安全隐患排查</p>
      <button class="startbtn" @click="url = srcTour">开始</button>
    </v-pannellum>
    <!-- // 控制器 -->
    <div class="controls">
      <label>
        <span>Type:</span>
        <button @click="url = equirectangularUrl">equirect</button>
        <button @click="url = cubemapUrls">cubemaps</button>
        <button @click="url = srcTour">scenes</button>
      </label>
      <label>
        <span>Hfov:</span>
        <input type="text" v-model.number="hfov" style="width: 50px;" />
      </label>
      <label>
        <span>Yaw:</span>
        <input type="text" v-model.number="yaw" style="width: 50px;" />
      </label>
      <label>
        <span>Pitch:</span>
        <input type="text" v-model.number="pitch" style="width: 50px;" />
      </label>
      <label>
        <input type="checkbox" v-model="show" />
        <span>Show</span>
      </label>
      <label>
        <input type="checkbox" v-model="isAutoRotationOn" />
        <span>Auto Rotation</span>
      </label>
      <label>
        <input type="checkbox" v-model="isOrientationOn" />
        <span>Orientation</span>
      </label>
    </div>
    <!-- // 抽屉 -->
    <div :class="['infodrawer', isDrawerShow ? 'drawershow' : '']">
      <div class="drawer-header">
        <button type="button" class="drawer-close-btn" @click="onClose()">
          &times;
        </button>
        <h3 class="drawer-title">{{ drawerTitle }}</h3>
      </div>
      <div className="drawer-body">
          <img
            :src="pic"
            loading="lazy"
            className="drawer-body__img"
          />
          <div className="drawer-body__text">
            <p>
              {{ msg }}
            </p>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
import equirectangularUrl from './equirectangular/wooden-lounge.png'
import equirectangularUrlLivingroom from './equirectangular/livingroom.jpg'
import equirectangularUrlKitchen from './equirectangular/kitchen.jpg'
import equirectangularUrlLiftroom from './equirectangular/liftroom.jpg'
import equirectangularUrlFrontdoor from './equirectangular/frontdoor.jpg'

import px from './cubemaps/px.jpg'
import nx from './cubemaps/nx.jpg'
import py from './cubemaps/py.jpg'
import ny from './cubemaps/ny.jpg'
import pz from './cubemaps/pz.jpg'
import nz from './cubemaps/nz.jpg'

export default {
  data() {
    return {
      // drawer
      isDrawerShow: false,
      drawerTitle: '',
      msg: '',
      pic: '',

      show: true,
      hfov: 60,
      yaw: 180,
      pitch: 0,
      url: equirectangularUrl,
      equirectangularUrl,
      cubemapUrls: { pz, px, nz, nx, py, ny },
      isAutoRotationOn: false,
      isOrientationOn: false,
      hotSpots: [
      ],
      srcTour: {
        default: {
          firstScene: 'liftroom',
          author: 'Foo Bar',
          sceneFadeDuration: 1000,
        },
        scenes: {
          kitchen: {
            title: 'Kitchen',
            hfov: 60,
            pitch: -3,
            yaw: 117,
            type: 'equirectangular',
            panorama: equirectangularUrlKitchen,
            hotSpots: [
              {
                pitch: -0.6,
                yaw: 107.1,
                type: 'scene',
                text: '客厅',
                sceneId: 'livingroom',
                targetYaw: -23,
                targetPitch: 2,
              },
              {
                pitch: 12,
                yaw: -86,
                type: 'info',
                text: '电器未使用 电源线未拔下',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: 7,
                yaw: -71,
                type: 'info',
                text: '热水器排气口离电源过近',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: 5,
                yaw: -140,
                type: 'info',
                text: '加热类电器距离过近',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
            ],
          },
          liftroom: {
            title: 'Liftroom',
            hfov: 60,
            pitch: -3,
            yaw: 180,
            type: 'equirectangular',
            panorama: equirectangularUrlLiftroom,
            hotSpots: [
              {
                pitch: -20,
                yaw: 180,
                type: 'scene',
                text: '前进',
                sceneId: 'frontdoor',
                targetYaw: 180,
                targetPitch: 0,
              },
              {
                pitch: -13,
                yaw: -162,
                type: 'info',
                text: '灭火器',
              },
              {
                pitch: -13,
                yaw: 162,
                type: 'info',
                text: '灭火器',
              },
            ],
          },
          frontdoor: {
            title: 'Frontdoor',
            hfov: 60,
            pitch: -3,
            yaw: 117,
            type: 'equirectangular',
            panorama: equirectangularUrlFrontdoor,
            hotSpots: [
              {
                pitch: -10,
                yaw: -10,
                type: 'scene',
                text: '前进',
                sceneId: 'liftroom',
                targetYaw: -23,
                targetPitch: 2,
              },
              {
                pitch: -20,
                yaw: 20,
                type: 'info',
                text: '灭火器',
              },
              {
                pitch: 1,
                yaw: 141,
                type: 'scene',
                text: '进入室内',
                sceneId: 'livingroom',
                targetYaw: 50,
                targetPitch: -8,
              },
              {
                pitch: 15,
                yaw: -99,
                type: 'info',
                text: '常闭式防火门',
              },
              {
                pitch: 0,
                yaw: -95,
                type: 'info',
                text: '常闭式防火门未正常关闭',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: -15,
                yaw: -79,
                type: 'info',
                text: '消防通道内未发现杂物',
                cssClass: "right-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: -20,
                yaw: -27,
                type: 'info',
                text: '灭火器',
              },
              {
                pitch: 30,
                yaw: 55.8,
                type: 'info',
                text: '手动报警器',
                // clickHandlerFunc: 'setShowDrawer("handwarning")',
              },
              {
                pitch: 24,
                yaw: 65,
                type: 'info',
                text: '隐藏式消防栓',
              },
              {
                pitch: -10,
                yaw: 75,
                type: 'info',
                text: '消防栓旁堆放过多物品',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
            ],
          },
          livingroom: {
            title: 'Livingroom',
            hfov: 60,
            pitch: -3,
            yaw: 117,
            type: 'equirectangular',
            panorama: equirectangularUrlLivingroom,
            hotSpots: [
              {
                pitch: -2.5,
                yaw: 52,
                type: 'info',
                text: '电视电源未关闭',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: 21,
                yaw: 66,
                type: 'info',
                text: '电灯电源未关闭',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: -20,
                yaw: -143,
                type: 'info',
                text: '电源线杂乱',
                cssClass: "warning-hotspot",
                // clickHandlerFunc: () => setShowDrawer(DRAWER_TYPES.CEREMONY)
              },
              {
                pitch: 0,
                yaw: -177,
                type: 'scene',
                text: '门口',
                sceneId: 'frontdoor',
                targetYaw: -23,
                targetPitch: 2,
              },
              {
                pitch: 0,
                yaw: -57,
                type: 'scene',
                text: '厨房',
                sceneId: 'kitchen',
                targetYaw: -23,
                targetPitch: 2,
              },
            ],
          },
        },
      },
    }
  },
  methods: {
    onClose() {
      this.isDrawerShow = false;
    },
    setShowDrawer(type) {
      switch(key) {
        case 'handwarning':
          this.msg = '火灾报警系统中的一个设备类型，当人员发现火灾时，在火灾探测器没有探测到火灾的时候，人员手动按下手动报警按钮，报告火灾信号。正常情况下当手动报警按钮报警时，火灾发生的几率比火灾探测器要大的多，几乎没有误报的可能。因为手动报警按钮的报警出发条件是必须人工按下按钮启动。按下手动报警按钮的的时候过3-5秒钟手动报警按钮上的火警确认灯会点亮，这个状态灯表示火灾报警控制器已经收到火警信号，并且确认了现场位置。';
          this.pic = './img/sdbjq.jpg';
      };
      this.isDrawerShow = true;
    },
  }
}
</script>

<style>
html,
body,
.app,
.pannellum {
  height: 100%;
}
.topic {
  font-size: 48px;
  text-align: center;
  margin-top: 20%;
  margin-bottom: 40%;
  width: 100vw;
  font-weight: 600;
  color: aquamarine;
}
.startbtn {
  display: block;
  margin: auto;
  padding: 30px;
  font-size: 36px;
  color: #fff;
  background: #07aff7;
  border: 0;
  border-radius: 5px;
  width: 30%;
  max-width: 240px;
}
.warning-hotspot {
  height: 40px;
  width: 40px;
  opacity: 0.9;
  background-image: url("./img/redwarning.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
}

.warning-hotspot span, .right-hotspot span {
  background: #fff !important;
  color: #000 !important;
}

.right-hotspot {
  height: 40px;
  width: 40px;
  opacity: 0.9;
  background-image: url("./img/right.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
}

.infodrawer {
  max-height: 92vh;
  width: 100vw;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  background: #fff;
  border-radius: 8px 8px 0 0;
  transform: translateY(100%);
  transition: 0.2s;
}
.drawershow {
  transform: translateY(0);
}

.drawer-header {
  position: relative;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
}

.drawer-header .drawer-close-btn {
  border: none;
  background: rgba(0, 0, 0, 0.25);
  border-radius: 100%;
  height: 32px;
  width: 32px;
  font-size: 24px;
  margin: 16px;
  cursor: pointer;
}

.drawer-body {
  display: flex;
  padding: 32px;
  gap: 24px;
}

.drawer-body__img {
  border-radius: 16px;
  width: 40%;
  max-width: 400px;
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.25);
  height: 100%;
}

.drawer-body__text {
  line-height: 1.5;
}

body {
  margin: 0;
}

.controls {
  position: fixed;
  left: 10px;
  bottom: 10px;
  padding: 5px;
  border: solid 1px silver;
  background-color: hsla(0, 0%, 100%, 0.3);
  z-index: 10;
}
</style>
