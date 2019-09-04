<template lang='pug'>
  #app
    #container(v-show="viewmap")
    #chairs(v-show="viewSeat")
    button(@click="changeStates" v-show="viewSeat") back
</template>

<script>
import VueAnychart from './components/Maps'
import Anychart from 'anychart'

export default {
  name: 'App',
  components: {
    VueAnychart
  },
  data () {
    return {
      Anychart: Anychart,
      map: '',
      viewmap: true,
      viewSeat: false
    }
  },
  created () {
    this.getMap()
  },
  methods: {
    changeStates () {
      this.viewmap = true
      this.viewSeat = false
    },
    getMap () {
      this.$http.get('https://cors-anywhere.herokuapp.com/https://codessign.com/wp-content/uploads/2019/09/mapa-colorf.svg')
        .then((res) => {
          this.map = res.data
          this.createSeatMap()
        })
        .catch((error) => {
          console.log(error)
        })
    },
    getChairs () {
      this.$http.get('https://static.anychart.com/images/docs/seat_map/theatre_seating_plan.svg')
        .then((res) => {
          this.chairs = res.data
          this.createChairs()
          this.viewmap = false
          this.viewSeat = true
        })
        .catch((error) => {
          console.log(error)
        })
    },
    createSeatMap () {
      var self = this
      // create a new seat map
      var chart = Anychart.seatMap()
      // load SVG data
      chart.geoData(this.map)
      let seatMapSeries = chart.choropleth()
      // load data
      seatMapSeries.data([
        {id: 'locacion1', value: '100'},
        {id: 'locacion2', value: '265'},
        {id: 'locacion3', value: '490'},
        {id: 'locacion4', value: '108', fill: 'red 0.5', hoverFill: 'green 0.1', hoverStroke: '3 green'}
      ])
      // chart.interactivity(false)
      // chart.interactivity().zoomOnMouseWheel(true)
      // chart.interactivity().keyboardZoomAndMove(true)
      // chart.interactivity().zoomOnDoubleClick(true)

      var clicked = true
      chart.listen('click', function (evt) {
        if (clicked) {
          chart.zoomTo(3, evt.clientX, evt.clientY)
        } else {
          chart.zoomTo(0, evt.clientX, evt.clientY)
        }
        clicked = !clicked
      })

      chart.listen('pointClick', function (evt) {
        var point = evt.point
        var id = point.get('id') // trae el key del objeto de la grafica
        console.log(id)
        self.getChairs()
        // this.$http.get(url)
        //   .then((res) => {
        //     console.log(res)
        // var geoData = 'anychart.maps.' + urlName
        // var drillMap = anychart.map()
        // var drillDownTitle = 'Orders from ACME Online Shop in <span style="color:#212121 font-weight: bold">' + name + '</span><br/><span style="color:#212121 font-size: 13px">Average number of orders by month. <br> To drill up Press Backspace button or use context menu</span>'
        // drillMap.padding([10, 10, 10, 10])
        // drillMap.title().enabled(true).padding([0, 0, 20, 0]).useHtml(true).text(drillDownTitle)
        // drillMap.geoData(geoData)

        // var dataSet = anychart.data.set(regions_data[id]).mapAs()
        // var series = drillMap.choropleth(dataSet)
        // series.stroke('#fff9c4')
        // series.colorScale(anychart.scales.linearColor('#fff9c4', '#fbc02d', '#e64a19', '#dd2c00'))
        // series.tooltip().titleFormat(function () {
        //     return this.getData('name')
        // })
        // series.tooltip().enabled(true).useHtml(true).padding([8, 13, 10, 13]).fontSize(13).format(function () {
        //     return '<span style="font-size: 12px color: #cbcbcb">Orders:</span> ' + this.getData("value") + '<span style="font-size: 12px color: #cbcbcb"></span>'
        // })
        // map.drillTo(id, drillMap)
      // })
      // .catch((error) => {
      //   console.log('Error: ', error)
      // })
      })
      // load svg-file how it looked(colors stroke/fill except for elements of series)
      // chart.unboundRegions('as-is')
      // set container id for the chart
      chart.container('container')
      // initiate chart drawing
      chart.draw()
    },
    createChairs () {
      document.getElementById('chairs').innerHTML = ''
      // create a new seat map
      var chart = Anychart.seatMap()
      // load SVG data
      chart.geoData(this.chairs)
      // set the title
      // chart.title('Load SVG map through the AJAX request')
      // create new series
      // palete
      var palette = Anychart.palettes.rangeColors()
      palette.items(['#F44336', '#FCEF86'])
      palette.count(8)

      // setting custom colors to the chart palette
      chart.palette(palette)

      // setting the data
      chart.choropleth([
        {id: 'r1s1', value: 12},
        {id: 'r1s2', value: 12},
        {id: 'r1s3', value: 12},
        {id: 'r1s4', value: 12},
        {id: 'r1s5', value: 12},
        {id: 'r1s6', value: 12}])
        .name('Row 1')

      chart.choropleth([
        {id: 'r2s1', value: 12},
        {id: 'r2s2', value: 12},
        {id: 'r2s3', value: 12},
        {id: 'r2s4', value: 12},
        {id: 'r2s5', value: 12},
        {id: 'r2s6', value: 12},
        {id: 'r2s7', value: 12},
        {id: 'r2s8', value: 12}])
        .name('Row 2')

      chart.choropleth([
        {id: 'r3s1', value: 5.5},
        {id: 'r3s2', value: 10},
        {id: 'r3s3', value: 10},
        {id: 'r3s4', value: 10},
        {id: 'r3s5', value: 10},
        {id: 'r3s6', value: 5.5}])
        .name('Row 3')

      chart.choropleth([
        {id: 'r4s1', value: 5.5},
        {id: 'r4s2', value: 5.5},
        {id: 'r4s3', value: 10},
        {id: 'r4s4', value: 10},
        {id: 'r4s5', value: 10},
        {id: 'r4s6', value: 10},
        {id: 'r4s7', value: 5.5},
        {id: 'r4s8', value: 5.5}])
        .name('Row 4')

      chart.choropleth([
        {id: 'r5s1', value: 5.5},
        {id: 'r5s2', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s3', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s4', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s3', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s4', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s5', value: 10},
        {id: 'r5s6', value: 10},
        {id: 'r5s7', value: 10},
        {id: 'r5s8', value: 10},
        {id: 'r5s9', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s10', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s11', value: 3.5, extra: 'RESTRICTED VIEW'},
        {id: 'r5s12', value: 5.5}])
        .name('Row 5')

      chart.choropleth([
        {id: 'r6s1', value: 5.5},
        {id: 'r6s2', value: 5.5},
        {id: 'r6s3', value: 7.5},
        {id: 'r6s4', value: 7.5},
        {id: 'r6s5', value: 10},
        {id: 'r6s6', value: 10},
        {id: 'r6s7', value: 10},
        {id: 'r6s8', value: 10},
        {id: 'r6s9', value: 7.5},
        {id: 'r6s10', value: 7.5},
        {id: 'r6s11', value: 5.5},
        {id: 'r6s12', value: 5.5}])
        .name('Row 6')

      chart.choropleth([
        {id: 'r7s1', value: 5.5},
        {id: 'r7s2', value: 5.5},
        {id: 'r7s3', value: 7.5},
        {id: 'r7s4', value: 7.5},
        {id: 'r7s5', value: 10},
        {id: 'r7s6', value: 10},
        {id: 'r7s7', value: 10},
        {id: 'r7s8', value: 10},
        {id: 'r7s9', value: 7.5},
        {id: 'r7s10', value: 7.5},
        {id: 'r7s11', value: 5.5},
        {id: 'r7s12', value: 5.5}])
        .name('Row 7')

      chart.choropleth([
        {id: 'r8s1', value: 5.5},
        {id: 'r8s2', value: 5.5},
        {id: 'r8s3', value: 7.5},
        {id: 'r8s4', value: 7.5},
        {id: 'r8s5', value: 10},
        {id: 'r8s6', value: 10},
        {id: 'r8s7', value: 10},
        {id: 'r8s8', value: 10},
        {id: 'r8s9', value: 7.5},
        {id: 'r8s10', value: 7.5},
        {id: 'r8s11', value: 5.5},
        {id: 'r8s12', value: 5.5}])
        .name('Row 8')
      // var clicked = true
      // chart.listen('click', function (evt) {
      //   if (clicked) {
      //     chart.zoomTo(3, evt.clientX, evt.clientY)
      //   } else {
      //     chart.zoomTo(0, evt.clientX, evt.clientY)
      //   }
      //   clicked = !clicked
      // })
      chart.listen('pointClick', function (evt) {
        // console.log(evt)
        console.log(evt.point.get('id'))
        var index = evt.iterator.getIndex()
        console.log(index)
        // var row = dataSet.row(index)
        // if (row.fillOld){
        //   row.fill = row.fillOld; delete row.fillOld
        // }else{
        //   row.fillOld = row.fill; row.fill = "green"
        // }
        // dataSet.row(index, row)
      })

      chart.tooltip().titleFormat('{%seriesName}')
      chart.tooltip().format('$ {%value} pesitos')
      chart.legend().enabled(true).position('bottom').padding({top: 15})
      chart.container('chairs')
      // initiate chart drawing
      chart.draw()
    }
  }
}
</script>

<style lang='css'>

html, body, #container {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
}

svg {
  height: 90vh;
}
</style>
