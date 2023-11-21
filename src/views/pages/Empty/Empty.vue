<template>
    <div class="body">
      
      <div ref="map" id="map"></div>
      <Button @click="toggle" class="p-button p-button-secondary mr-2 mb-2"> Legenda</Button>
      <OverlayPanel ref="op" appendTo="body" :showCloseIcon="false">
        <Tree 
          v-model:selectionKeys="selectedKey" 
          :value="nodes" 
          selectionMode="checkbox" 
          class="w-full md:w-30rem"
          @nodeSelect="onNodeSelect"
          @nodeUnselect="onNodeUnselect"
          />
      </OverlayPanel>
      <pre id="info"></pre>
      <!-- <Button @click="isMenu1Open=!isMenu1Open" class="btn">Tombol</Button> -->
    </div>
    
    <Menu1 :propsVisible="openMenu1" @closeModal="(closeModal)=>closeMenu1(closeModal)"></Menu1>
    <!-- <Dialog v-model:visible="visible" maximizable header="Header" :style="{ width: '50vw' }">
        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
            Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
    </Dialog> -->
  </template>
  
  <script>
  // import * as mapboxgl from 'mapbox-gl';
  import * as maplibregl from 'maplibre-gl';
  import 'maplibre-gl/dist/maplibre-gl.css'
  import Menu1 from './Menu1/Menu1.vue';
  export default {
    data() {
      return {
        map: null,
        mapku:null,
        marker:{},
        nodes:null,
        selectedKey: null
        // op:null
        // visible:true
      };
    },
    emits: ['closeMenu1'],
    props:{
      openMenu1:Boolean,
      toggleJalan:Boolean,
      togglePelabuhan:Boolean,
      togglePolygon:Boolean,
      toggleWaisai:Boolean
    },
    components:{
      Menu1
    },
    mounted(){
      // NodeService.getTreeNodes().then((data) => (console.log(data)));
      this.getTreeNodes().then((data)=>{
        this.nodes = data
        console.log(data)
      })
      this.mapku = new maplibregl.Map({
            container: this.$refs.map, // container id
            style: {
                'version': 8,
                'sources': {
                    'raster-tiles': {
                        'type': 'raster',
                        'tiles': [
                            'https://api.maptiler.com/maps/satellite/256/{z}/{x}/{y}.jpg?key=I2yXUrRywAILTorOi6nw'
                        ],
                        'tileSize': 256,
                        
                    }
                },
                'layers': [
                    {
                        'id': 'simple-tiles',
                        'type': 'raster',
                        'source': 'raster-tiles',
                        'minzoom': 1,
                        'maxzoom': 21
                    }
                ]
            },
            maxZoom: 21,
            minZoom: 13,
            center: [130.814963, -0.423930], // starting position
            zoom: 15 // starting zoom
        });

        
        this.mapku.addControl(new maplibregl.ScaleControl());
        this.mapku.on('mousemove', (e) => {
          document.getElementById('info').innerHTML =
          `Posisi Mouse: ${e.lngLat.lng}, ${e.lngLat.lat}`
          });
        this.mapku.on('load', () => {
          // // source raster waisai
          // this.mapku.addSource('waisai-raster', {
          //   'type': 'raster',
          //   'tiles': ['http://localhost:8000/services/original1/tiles/{z}/{x}/{y}.png'], 
          //   'tileSize': 256 
            
          // });
          
          this.mapku.addSource('places', {
                'type': 'geojson',
                'data': {
                    'type': 'FeatureCollection',
                    'features': [
                        {
                            'type': 'Feature',
                            'properties': {
                                'description':
                                    '<strong>Ini Kucing Oren</strong><p>Lorem Ipsum Dolor Sit Amet bla bla bla</p>',
                                'icon': 'theatre'
                            },
                            'geometry': {
                                'type': 'Point',
                                'coordinates': [130.805657,-0.429650]
                            }
                        }
                    ]
                }
            });

            // Lines
            // this.drawLine(this.mapku)
            // End Lines
            
            // Gambar kucing
            this.mapku.loadImage(
                'https://upload.wikimedia.org/wikipedia/commons/7/7c/201408_cat.png',
                (error, image) => {
                    if (error) throw error;
                    this.mapku.addImage('cat', image);
                    this.mapku.addSource('point', {
                        'type': 'geojson',
                        'data': {
                            'type': 'FeatureCollection',
                            'features': [
                                {
                                    'type': 'Feature',
                                    'geometry': {
                                        'type': 'Point',
                                        'coordinates': [130.814963, -0.423930]
                                    }
                                }
                            ]
                        }
                    });
                }
            );
            

          //   this.mapku.addLayer({
          //   'id': 'waisai-raster-layer', 
          //   'type': 'raster', 
          //   'source': 'waisai-raster',
          //   'paint': {}
          // });


            // Add a layer showing the places.
            this.mapku.addLayer({
                'id': 'places',
                'type': 'symbol',
                'source': 'places',
                'layout': {
                    'icon-image': 'cat',
                    'icon-size': 0.12,
                    'icon-overlap': 'always'
                }
            });
            
            // this.mapku.on('click', 'lines',(e)=>{
            //   console.log(e)
            //   console.log(e.features)
            // })
            this.mapku.on('click', 'places', (e) => {
                const coordinates = e.features[0].geometry.coordinates.slice();
                const description = e.features[0].properties.description;

                // Ensure that if the map is zoomed out such that multiple
                // copies of the feature are visible, the popup appears
                // over the copy being pointed to.
                while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
                    coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
                }

                new maplibregl.Popup()
                    .setLngLat(coordinates)
                    .setHTML(description)
                    .addTo(this.mapku);
            });

            // Change the cursor to a pointer when the mouse is over the places layer.
            this.mapku.on('mouseenter', 'places', () => {
              this.mapku.getCanvas().style.cursor = 'pointer';
            });

            // Change it back to a pointer when it leaves.
            this.mapku.on('mouseleave', 'places', () => {
              this.mapku.getCanvas().style.cursor = '';
            });
        });
        
        // map.scrollZoom.disable();
        this.mapku.addControl(new maplibregl.NavigationControl());
        this.mapku.addControl(new maplibregl.FullscreenControl());
    },
    methods: {
      onNodeSelect(selected){
        if(selected.children){
          selected.children.forEach(element => {
            this.addGeojson(element)
          });
        }
        else{
          this.addGeojson(selected)
          
        }
      },
      onNodeUnselect(unselected){
        if (unselected.children) {
          unselected.children.forEach(element => {
            this.removeGeojson(element)
          });
        }
        else{
          this.removeGeojson(unselected)
        } 
      },
      getTreeNodes() {
        return fetch('http://localhost:5000/api/simtaru')
            .then((res) => res.json())
            .then((d) => d.root);
      },
      toggle(event) {
        this.$refs.op.toggle(event);
      },
      closeMenu1(close){
        this.$emit('closeMenu1',close)
      },
      addGeojson(selected){
        this.mapku.addSource(selected.data, {
            'type': 'geojson',
            'data': `http://localhost:5000/api/simtaru/${selected.parent}/${selected.data}`
          });

          this.mapku.addLayer({
                  'id': `${selected.data}`,
                  'type': 'fill',
                  'source': `${selected.data}`,
                  'paint': {
                    'fill-color': '#2b0575',
                    'fill-opacity': 0.8
                  }

              })
      },
      removeGeojson(unselected){
          this.mapku.removeLayer(unselected.data);
          this.mapku.removeSource(unselected.data);
      },
      drawLine(){
        this.mapku.addSource('lines', {
            'type': 'geojson',
            'data': {
                'type': 'FeatureCollection',
                'features': [
                    {
                        'type': 'Feature',
                        'properties': {
                            'color': '#F7455D' // red
                        },
                        'geometry': {
                            'type': 'LineString',
                            'coordinates': [
                                          [
                                            130.80494145584498,
                                            -0.43270504020748035
                                          ],
                                          [
                                            130.80503317745155,
                                            -0.43230950205017393
                                          ],
                                          [
                                            130.80517649246087,
                                            -0.43177351915142026
                                          ],
                                          [
                                            130.80524528366539,
                                            -0.431415241791953
                                          ],
                                          [
                                            130.80524528366539,
                                            -0.43097671028097295
                                          ],
                                          [
                                            130.8056150359031,
                                            -0.4305066512646789
                                          ],
                                          [
                                            130.80592459632402,
                                            -0.43012544408964004
                                          ],
                                          [
                                            130.80628001778496,
                                            -0.4297299060110191
                                          ],
                                          [
                                            130.80661824120853,
                                            -0.42939169209391537
                                          ],
                                          [
                                            130.8070453199374,
                                            -0.4289904213247979
                                          ],
                                          [
                                            130.8073720591966,
                                            -0.42873815827040573
                                          ],
                                          [
                                            130.8075211067237,
                                            -0.4286349742966564
                                          ],
                                          [
                                            130.80801697665697,
                                            -0.42823370348781964
                                          ],
                                          [
                                            130.80874501690698,
                                            -0.4276604594385418
                                          ],
                                          [
                                            130.80895998942202,
                                            -0.42743976046875787
                                          ],
                                          [
                                            130.8093784663364,
                                            -0.42704422103682305
                                          ],
                                          [
                                            130.80987433626973,
                                            -0.4265827595097136
                                          ],
                                          [
                                            130.8101351695871,
                                            -0.426321933417114
                                          ],
                                          [
                                            130.81016648888215,
                                            -0.42625041422282095
                                          ],
                                          [
                                            130.81018137503418,
                                            -0.426174632272307
                                          ],
                                          [
                                            130.81022332691805,
                                            -0.4261069698160753
                                          ],
                                          [
                                            130.81029911096527,
                                            -0.42606095934516475
                                          ],
                                          [
                                            130.81040331402966,
                                            -0.4260190086220348
                                          ],
                                          [
                                            130.81049127765658,
                                            -0.4260027696317792
                                          ],
                                          [
                                            130.8105711215632,
                                            -0.4260081826285216
                                          ],
                                          [
                                            130.81065773190318,
                                            -0.4259797643960894
                                          ],
                                          [
                                            130.81100704857374,
                                            -0.42586033796145273
                                          ],
                                          [
                                            130.81137823223605,
                                            -0.42573287783979197
                                          ],
                                          [
                                            130.81182807800832,
                                            -0.4255887905919593
                                          ],
                                          [
                                            130.8127172262959,
                                            -0.42529710176491164
                                          ],
                                          [
                                            130.81390861471021,
                                            -0.4248578113418944
                                          ],
                                          [
                                            130.81505381585703,
                                            -0.4244014722917342
                                          ],
                                          [
                                            130.81613625724907,
                                            -0.42402192532284744
                                          ],
                                          [
                                            130.8172046437952,
                                            -0.42359668808386175
                                          ],
                                          [
                                            130.8180797344021,
                                            -0.4233155421545405
                                          ],
                                          [
                                            130.81871233001942,
                                            -0.42305548216017996
                                          ],
                                          [
                                            130.81972282176247,
                                            -0.4223535741016917
                                          ],
                                          [
                                            130.8207947198951,
                                            -0.42172802429442413
                                          ],
                                          [
                                            130.82181428622135,
                                            -0.42106835515140517
                                          ],
                                          [
                                            130.82342875897774,
                                            -0.4200469815344121
                                          ],
                                          [
                                            130.82347269021028,
                                            -0.41997010394496215
                                          ],
                                          [
                                            130.823593501096,
                                            -0.4199206826377093
                                          ],
                                          [
                                            130.82365390653888,
                                            -0.4199206826377093
                                          ],
                                          [
                                            130.82370332917543,
                                            -0.41993715640650464
                                          ],
                                          [
                                            130.82469727328726,
                                            -0.4197779099680332
                                          ],
                                          [
                                            130.82661206841715,
                                            -0.41948223409249863
                                          ]
                                        ]
                        }
                    },
                    {
                        'type': 'Feature',
                        'properties': {
                            'color': '#33C9EB' // blue
                        },
                        'geometry': {
                            'type': 'LineString',
                            'coordinates': [
                                          [
                                            130.8057140534766,
                                            -0.4301262173882918
                                          ],
                                          [
                                            130.80560693475468,
                                            -0.4300893963655028
                                          ],
                                          [
                                            130.80544960413192,
                                            -0.43004922797551615
                                          ],
                                          [
                                            130.80528223112992,
                                            -0.4300191016840387
                                          ],
                                          [
                                            130.80516172256966,
                                            -0.4300023648547864
                                          ],
                                          [
                                            130.80501108686718,
                                            -0.4299755859289718
                                          ],
                                          [
                                            130.80485710370345,
                                            -0.4299588490996058
                                          ],
                                          [
                                            130.80473994260228,
                                            -0.42993541753953934
                                          ],
                                          [
                                            130.8046194340398,
                                            -0.4298919017839751
                                          ],
                                          [
                                            130.8044922305594,
                                            -0.4297914808083476
                                          ],
                                          [
                                            130.80434159485685,
                                            -0.42965089144047397
                                          ],
                                          [
                                            130.8041608320143,
                                            -0.42951699680219235
                                          ],
                                          [
                                            130.80401689123204,
                                            -0.4293864495276125
                                          ],
                                          [
                                            130.80389303521065,
                                            -0.42929607064381514
                                          ],
                                          [
                                            130.8038026537883,
                                            -0.42923247068749504
                                          ],
                                          [
                                            130.8037223147474,
                                            -0.4291420918018929
                                          ],
                                          [
                                            130.8036654079263,
                                            -0.42904167081644573
                                          ],
                                          [
                                            130.80364197570646,
                                            -0.42892451299908885
                                          ],
                                          [
                                            130.80363528078624,
                                            -0.42881404991163663
                                          ],
                                          [
                                            130.80367210284652,
                                            -0.42868684999156415
                                          ],
                                          [
                                            130.80375578934866,
                                            -0.42851613430485713
                                          ],
                                          [
                                            130.80383947699806,
                                            -0.42833311726438694
                                          ],
                                          [
                                            130.8039097736575,
                                            -0.42814566473759896
                                          ],
                                          [
                                            130.80399011269844,
                                            -0.4279816437734212
                                          ],
                                          [
                                            130.8041708755431,
                                            -0.4277874965043935
                                          ],
                                          [
                                            130.80435498584467,
                                            -0.42754313872771377
                                          ],
                                          [
                                            130.80449223170666,
                                            -0.4273456440805745
                                          ],
                                          [
                                            130.80434397079432,
                                            -0.42722270376040683
                                          ],
                                          [
                                            130.80414190422016,
                                            -0.42709010125531677
                                          ],
                                          [
                                            130.80379460229352,
                                            -0.42683121064376905
                                          ],
                                          [
                                            130.8036872544248,
                                            -0.426749123375032
                                          ]
                                        ]
                        }
                    }
                ]
            }
        });
        this.mapku.addLayer({
                  'id': 'lines',
                  'type': 'line',
                  'source': 'lines',
                  'paint': {
                      'line-width': 6,
                      'line-color': ['get', 'color']
                  }
              });
      },

      removeLine(id){
          this.mapku.removeLayer(id);
          this.mapku.removeSource(id);
      },
      

      addFotoUdara(){
                  // source raster waisai
          this.mapku.addSource('waisai-raster', {
            'type': 'raster',
            'tiles': ['https://apisimtaru.kartabhumi.co.id/services/original1/tiles/{z}/{x}/{y}.png'], 
            'tileSize': 256 
            
          });
          


            this.mapku.addLayer({
            'id': 'waisai-raster', 
            'type': 'raster', 
            'source': 'waisai-raster',
            'paint': {}
          });
      },

      removeFotoUdara(){
        const id='waisai-raster'
        this.mapku.removeLayer(id);
        this.mapku.removeSource(id);
        
      },

      addMarker(coordinates,id){
        this.marker[id] = new maplibregl.Marker()
            .setLngLat(coordinates)
            .addTo(this.mapku);
      },

      removeMarker(id){
        this.marker[id].remove();
        delete this.marker[id]
      },

      addPolygon(){
        this.mapku.addSource('polygon', {
            'type': 'geojson',
            'data': {
                'type': 'FeatureCollection',
                'features': [
                    {
                        'type': 'Feature',
                        // 'properties': {
                        //     'color': '#F7455D' // red
                        // },
                        'geometry': {
                            'type': 'Polygon',
                            'coordinates': [
                                        [
                                          [
                                            130.80576508064962,
                                            -0.4300936441887018
                                          ],
                                          [
                                            130.80668380283504,
                                            -0.4292243401125404
                                          ],
                                          [
                                            130.80766179741926,
                                            -0.4285526050765611
                                          ],
                                          [
                                            130.80701967976262,
                                            -0.4281673452509125
                                          ],
                                          [
                                            130.8065356218372,
                                            -0.42772281465727247
                                          ],
                                          [
                                            130.80625901730838,
                                            -0.427377068622647
                                          ],
                                          [
                                            130.80570580825076,
                                            -0.42682387493576357
                                          ],
                                          [
                                            130.80493526706323,
                                            -0.4267942395578501
                                          ],
                                          [
                                            130.80444133040442,
                                            -0.4273276763312168
                                          ],
                                          [
                                            130.80421411954256,
                                            -0.42768330082616046
                                          ],
                                          [
                                            130.80393751501379,
                                            -0.42798953301664255
                                          ],
                                          [
                                            130.80380909148272,
                                            -0.4283550359374715
                                          ],
                                          [
                                            130.80356212315257,
                                            -0.4287106603848372
                                          ],
                                          [
                                            130.80363127428478,
                                            -0.4290366494464024
                                          ],
                                          [
                                            130.80461914760235,
                                            -0.42990595354399375
                                          ],
                                          [
                                            130.80576508064962,
                                            -0.4300936441887018
                                          ]
                                        ]
                                      ]
                        }
                      }
                    ]
            }
            
      });
        
          this.mapku.addLayer({
              'id': 'polygon',
              'type': 'fill',
              'source': 'polygon',
              'layout': {},
              'paint': {
                  'fill-color': '#088',
                  'fill-opacity': 0.8
              }
          });
      },
      removePolygon(id){
          this.mapku.removeLayer(id);
          this.mapku.removeSource(id);
      },
    },
    watch:{
      toggleJalan: function(news,old){
        news?this.drawLine():this.removeLine('lines')
      },
      togglePelabuhan: function(news,old){
        news?this.addMarker([130.804953, -0.431823],'pelabuhan'):this.removeMarker('pelabuhan')
      },
      togglePolygon: function(news,old){
        news?this.addPolygon():this.removePolygon('polygon')
      },
      toggleWaisai: function(news,old){
        news?this.addFotoUdara():this.removeFotoUdara()
      }
    },
    beforeDestroy() {
      if (this.map) {
        this.map.remove();
      }
    }
  };
  </script>
  
<style>
    .body { margin: 0; padding: 0; }
    #map { position: absolute; left:0; top: 9%; bottom: 0; width: 100%; height:88%; }

    #info {
      display: table;
      position: absolute;
      margin: 0px auto;
      word-wrap: anywhere;
      white-space: pre-wrap;
      margin-top: 36.5%;
      /* padding: 10px; */
      padding-bottom: 5px;
      border: none;
      border-radius: 3px;
      font-size: 12px;
      text-align: center;
      color: #222;
      background: #fff;
    }
    .custom-control-button {
      background-color: white;
      border: none;
      border-radius: 3px;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);
      cursor: pointer;
      font-size: 14px;
      padding: 10px;
    }
</style>