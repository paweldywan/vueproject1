<template>
    <div>
        <ol-map :loadTilesWhileAnimating="true" :loadTilesWhileInteracting="true" style="height:400px">

            <ol-view :center="center"
                     :rotation="rotation"
                     :zoom="zoom"
                     :resolutions="resolutions"
                     :projection="projection"
                     :minZoom="minZoom"
                     :maxZoom="maxZoom" />

            <ol-tile-layer>
                <ol-source-osm />
            </ol-tile-layer>

            <ol-tile-layer>
                <ol-source-wmts
                    :attributions="attribution"
                    :url="url"
                    :matrixSet="matrixSet"
                    :format="format"
                    :layer="layerName"
                    :style="styleName"
                    :projection="projection"
                />
            </ol-tile-layer>

            <ol-mouseposition-control />
            <ol-fullscreen-control />
            <ol-scaleline-control />
            <ol-rotate-control />
            <ol-zoom-control />
            <ol-zoomslider-control />
        </ol-map>
    </div>    
</template>

<script lang="ts">
    import {
        onMounted,
        ref
    } from 'vue'

    import { WMTSCapabilities } from "ol/format.js";

    import { optionsFromCapabilities } from "ol/source/WMTS";

    export default {
        async setup() {
            const center = ref<(number | undefined)[]>([])
            const zoom = ref<number | undefined>(0)
            const rotation = ref(0)
            const url = ref('http://10.0.0.79/geoserver/gwc/service/wmts')
            const layerName = ref('vector_db:POLSKA')
            const matrixSet = ref('EPSG:4326')
            const format = ref('image/png')
            const styleName = ref('POLSKA')
            const attribution = ref('Tiles © <a href="https://services.arcgisonline.com/arcgis/rest/services/Demographics/USA_Population_Density/MapServer/">ArcGIS</a>')
            const projection = ref<any>();
            const resolutions = ref<number[]>();
            const extent = ref<any>();
            const maxZoom = ref<number>();
            const minZoom = ref<number>();
            const dimensions = ref<any>();

            const fetchData = async () => {
                const url2 = 'http://10.0.0.79/geoserver/gwc/service/wmts?SERVICE=WMTS&REQUEST=GetCapabilities';

                const response = await fetch(`httpclient?` + new URLSearchParams({
                    url: url2
                }));

                const data = await response.text();

                const parser = new WMTSCapabilities();

                const result = parser.read(data);

                const optionsToSet = optionsFromCapabilities(result, {
                    layer: layerName.value,
                    matrixSet: matrixSet.value
                });

                center.value = [optionsToSet?.tileGrid.getExtent()[0], optionsToSet?.tileGrid.getExtent()[1]];

                zoom.value = optionsToSet?.tileGrid.getMinZoom();

                projection.value = optionsToSet?.projection;

                resolutions.value = optionsToSet?.tileGrid.getResolutions();

                extent.value = optionsToSet?.tileGrid.getExtent();

                maxZoom.value = optionsToSet?.tileGrid.getMaxZoom();

                minZoom.value = optionsToSet?.tileGrid.getMinZoom();

                dimensions.value = optionsToSet?.dimensions;

                console.log(result);

                console.log(optionsToSet);
            };

            await fetchData();
             
            return {
                center,
                zoom,
                rotation,
                url,
                layerName,
                matrixSet,
                format,
                styleName,
                attribution,
                projection,
                resolutions,
                extent,
                maxZoom,
                minZoom,
                dimensions
            }
        },
    }
</script>