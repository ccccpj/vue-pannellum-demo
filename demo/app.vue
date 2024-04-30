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
      Slot content
    </v-pannellum>
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
      show: true,
      hfov: 90,
      yaw: -90,
      pitch: 0,
      url: equirectangularUrl,
      equirectangularUrl,
      cubemapUrls: { pz, px, nz, nx, py, ny },
      isAutoRotationOn: false,
      isOrientationOn: false,
      hotSpots: [
        {
          pitch: 14.1,
          yaw: 1.5,
          type: 'info',
          text: 'Click me to Google',
          URL: 'https://google.com/',
        },
        {
          pitch: 0,
          yaw: -90,
          type: 'info',
          text: 'I am <b>bold</b> text.',
        },
        {
          pitch: -0.9,
          yaw: 144.4,
          type: 'info',
          text: 'Info 2',
        },
      ],
      srcTour: {
        default: {
          firstScene: 'livingroom',
          author: 'Foo Bar',
          sceneFadeDuration: 1000,
        },
        scenes: {
          kitchen: {
            title: 'Kitchen',
            hfov: 110,
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
            ],
          },
          liftroom: {
            title: 'Liftroom',
            hfov: 110,
            pitch: -3,
            yaw: 117,
            type: 'equirectangular',
            panorama: equirectangularUrlLiftroom,
            hotSpots: [
              {
                pitch: -10,
                yaw: 180,
                type: 'scene',
                text: '前进',
                sceneId: 'frontdoor',
                targetYaw: 180,
                targetPitch: 0,
              },
            ],
          },
          frontdoor: {
            title: 'Frontdoor',
            hfov: 110,
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
            ],
          },
          livingroom: {
            title: 'Livingroom',
            hfov: 110,
            pitch: -3,
            yaw: 117,
            type: 'equirectangular',
            panorama: equirectangularUrlLivingroom,
            hotSpots: [
              {
                pitch: -0.6,
                yaw: -177.1,
                type: 'scene',
                text: '门口',
                sceneId: 'frontdoor',
                targetYaw: -23,
                targetPitch: 2,
              },
              {
                pitch: -0.6,
                yaw: -57.1,
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
}
</script>

<style>
html,
body,
.app,
.pannellum {
  height: 100%;
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
