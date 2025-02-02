<template>
  <div class='stationsMap'>
    <svg ref='svgTree'></svg>
    <infoBtn :content='infoData.content'/>
  </div>
</template>

<script>
import infoBtn from './InfoBtn'
const d3 = require('d3')

export default {
  name: 'StationsMap',
  components: {
    infoBtn: infoBtn
  },
  props: {},
  data () {
    return {
      activeStations: new Set([
        'Waterways', 'Kannika',
        'Boonsri', 'Kohsoom',
        'Busarakhan', 'Somchair',
        'Achara', 'Tansanee',
        'Chai', 'Decha', 'Sakda'
      ]),
      treeData: {
        name: 'Waterways',
        children: [
          {
            name: 'Kannika',
            children: [
              {
                name: 'Chai',
                children: [{ name: 'Boonsri' }, { name: 'Kohsoom' }]
              },
              { name: 'Busarakhan' }
            ]
          },
          {
            name: 'Sakda',
            children: [{ name: 'Somchair' }, { name: 'Achara' }]
          },
          { name: 'Tansanee' },
          { name: 'Decha' }
        ]
      },
      infoData: {
        title: '',
        content: 'This is a tree representing stations water flow.' +
        ' For example Boonsri and Kohsoom waters flow into Chai. <br>' +
        'This is an interactive map, click on stations to enable/disable it,' +
        ' it will alter the calendar visualization.'
      }
    }
  },
  mounted () {
    this.$emit('stations-active', Array.from(this.activeStations))
    this.redraw()
  },
  methods: {
    isStationActive: function (station) {
      return this.activeStations.has(station)
    },
    addActiveStation: function (station) {
      // metto attiva la stazione, ridisegno e notifico tutte le stazioni attive
      this.activeStations.add(station)
      this.makeToast('Added ' + station + ' station')
      this.redraw()
      this.$emit('stations-active', Array.from(this.activeStations))
    },
    removeActiveStation: function (station) {
      this.activeStations.delete(station)
      this.makeToast('Removed ' + station + ' station')
      this.redraw()
      this.$emit('stations-active', Array.from(this.activeStations))
    },
    toggleStation: function (station) {
      if (this.isStationActive(station)) {
        this.removeActiveStation(station)
      } else {
        this.addActiveStation(station)
      }
      this.redraw()
    },
    redraw: function () {
      // set the dimensions and margins of the diagram
      var margin = { top: 20, right: 0, bottom: 30, left: 0 }
      var width = 380 - margin.left - margin.right
      var height = 400 - margin.top - margin.bottom

      // declares a tree layout and assigns the size
      var treemap = d3.tree().size([width, height])

      //  assigns the data to a hierarchy using parent-child relationships
      var nodes = d3.hierarchy(this.treeData)

      // maps the node data to the tree layout
      nodes = treemap(nodes)
      nodes.y = height
      // append the svg obgect to the body of the page
      // appends a 'group' element to 'svg'
      // moves the 'group' element to the top left margin
      this.svg = d3
        .select(this.$refs.svgTree)
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
      var g = this.svg
        .append('g')
        .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')')
      this.svg = this.svg.node()

      // adds the links between the nodes
      // var link = g
      g
        .selectAll('.link')
        .data(nodes.descendants().slice(1))
        .enter()
        .append('path')
        .attr('class', 'link')
        .attr('d', function (d) {
          d.y = height - d.y
          return (
            'M' +
            d.x +
            ',' +
            d.y +
            'C' +
            d.x +
            ',' +
            (d.y + d.parent.y) / 2 +
            ' ' +
            d.parent.x +
            ',' +
            (d.y + d.parent.y) / 2 +
            ' ' +
            d.parent.x +
            ',' +
            d.parent.y
          )
        })

      // adds each node as a group
      var node = g
        .selectAll('.node')
        .data(nodes.descendants())
        .enter()
        .append('g')
        .attr(
          'class',
          d =>
            'node' +
            (d.parent ? '' : ' node--root') +
            (d.children ? ' node--internal' : ' node--leaf') +
            (this.isStationActive(d.data.name)
              ? ' node--active'
              : ' node--inactive')
        )
        .attr('transform', function (d) {
          return 'translate(' + d.x + ',' + d.y + ')'
        })
        .on('click', d => this.toggleStation(d.data.name))

      // adds the circle to the node
      node.append('circle').attr('r', 12)
      node.append('polygon')
        .attr('id', d => d.name + '-checkbox')
        .attr('points', '-10,-5 0,0 10,-5 0,5')
        .style('pointer-events', 'none')
        .style('fill', d => this.isStationActive(d.data.name)
          ? 'forestgreen' : 'none')

      // adds the text to the node
      node
        .append('text')
        .attr('dy', '.35em')
        .attr('y', 20)
        .style('text-anchor', 'middle')
        .text(function (d) {
          return d.data.name
        })
    },
    makeToast (txt, append = false) {
      this.$bvToast.toast(`${txt}`, {
        title: 'Stations map',
        autoHideDelay: 1500,
        appendToast: append
      })
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
svg >>> .node--root {
  display: none;
}

svg >>> .node circle {
  fill: #fff;
  stroke: steelblue;
  stroke-width: 3px;
  cursor: pointer;
}

svg >>> .node--inactive > circle {
  stroke: gray;
}

svg >>> .node text {
  font: 12px sans-serif;
  fill: #16316e;
  text-shadow: 0 1px 0 #fff, 0 -1px 0 #fff, 1px 0 0 #fff, -1px 0 0 #fff;
}

svg >>> .node--inactive > text {
  fill: gray;
}

svg >>> .link {
  fill: none;
  stroke: #ccc;
  stroke-width: 2px;
}
</style>
