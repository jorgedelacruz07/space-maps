<template>
  <div>
    <div id="map-wrap" style="height: 100vh">
      <no-ssr>
        <l-map :zoom="zoom" :center="center">
          <l-tile-layer :url="url" />
          <l-marker v-for="(marker, index) in markers" :lat-lng="marker">
            <l-popup :custom="true">
              <b-container>
                <b-row class="my-1">
                  <b-col sm="2">
                    <label for="polygon-mts">Muestras:</label>
                  </b-col>
                  <b-col sm="10">
                    <b-form-input id="polygon-mts" v-model="marker.samples" size="sm" />
                  </b-col>
                </b-row>
                <b-row class="my-1">
                  <b-col sm="2">
                    <label for="polygon-radio">Radio (m):</label>
                  </b-col>
                  <b-col sm="10">
                    <b-form-input id="polygon-radio" v-model="marker.radio" size="sm" />
                  </b-col>
                </b-row>
                <b-row>
                  <b-button @click="printConsole" variant="success">
                    Guardar
                  </b-button>
                </b-row>
                <b-row>
                  <b-button @click="removeMarker(index)" variant="danger">
                    Eliminar
                  </b-button>
                </b-row>
              </b-container>
            </l-popup>
          </l-marker>
          <l-polygon :lat-lngs="positions" :color="color" @click="addMarker" />
        </l-map>
      </no-ssr>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      zoom: 17,
      center: [
        -75.80318792584497,
        -14.010508830892729
      ],
      // positions: [
      //   [
      //     -75.80318792584497,
      //     -14.010508830892729
      //   ],
      //   [
      //     -75.80357604197151,
      //     -14.010098485803613
      //   ],
      //   [
      //     -75.80341123599916,
      //     -14.008264647434418
      //   ],
      //   [
      //     -75.80337347726918,
      //     -14.00830884788491
      //   ],
      //   [
      //     -75.8032888725298,
      //     -14.00845576747385
      //   ],
      //   [
      //     -75.80314660182964,
      //     -14.00875662139284
      //   ],
      //   [
      //     -75.80301632487944,
      //     -14.008850357467912
      //   ],
      //   [
      //     -75.80283545211566,
      //     -14.008914402120103
      //   ],
      //   [
      //     -75.80272291099085,
      //     -14.009033547353713
      //   ],
      //   [
      //     -75.80266265796831,
      //     -14.009107336060822
      //   ],
      //   [
      //     -75.80255993524152,
      //     -14.009115853708527
      //   ],
      //   [
      //     -75.80185152559181,
      //     -14.009703043588672
      //   ],
      //   [
      //     -75.80228836137502,
      //     -14.009679340032271
      //   ],
      //   [
      //     -75.80222537770697,
      //     -14.010590959989404
      //   ],
      //   [
      //     -75.8026809899508,
      //     -14.010567922073863
      //   ],
      //   [
      //     -75.80318792584497,
      //     -14.010508830892729
      //   ]
      // ],
      positions: [],
      color: 'black',
      markers: []
    }
  },
  async asyncData ({ $axios }) {
    const data = await $axios.$get('https://analytics.spaceag.co/static/challenges_info/one_polygon_carto.json')
    const positions = data.features[0].geometry.coordinates
    return { positions }
  },
  methods: {
    addMarker (event) {
      this.markers.push(event.latlng)
    },
    removeMarker (index) {
      this.markers.splice(index, 1)
    },
    printConsole () {
      const markers = []
      this.markers.forEach((m) => {
        const data = {
          position: {
            latitude: m.lat,
            longuitude: m.lng
          },
          radio: m.radio,
          num_samples: m.samples
        }
        markers.push(data)
      })
      // eslint-disable-next-line no-console
      const data = {
        markers
      }
      console.log(data)
    }
  }
}
</script>
