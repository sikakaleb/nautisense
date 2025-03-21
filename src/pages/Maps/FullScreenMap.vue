<template>
  <div>
    <div id="map" class="full-screen-map"></div>
    <div class="legend-control">
      <h4>Légende</h4>
      <div class="legend-item">
        <div class="legend-color" style="background-color: #ff0000"></div>
        <span>Pollution</span>
      </div>
      <div class="legend-item">
        <div class="legend-color" style="background-color: #0000ff"></div>
        <span>Température Anormale</span>
      </div>
      <div class="legend-item">
        <div class="legend-color" style="background-color: #00ff00"></div>
        <span>Normal</span>
      </div>
      <hr style="margin: 8px 0" />
      <div class="legend-item">
        <div
          style="
            width: 16px;
            height: 16px;
            border: 1px solid #51bbd6;
            border-radius: 50%;
            margin-right: 8px;
          "
        ></div>
        <span>Cluster / Regroupement</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    mapboxgl.accessToken =
      "pk.eyJ1Ijoia2FsZWJzaWthMjAyNSIsImEiOiJjbThlZXhzN2owMHR2MmlzNWNxcHlqeDQ2In0.UvvmBCG_ANQnSkZ_vV-cmg";
    const map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/light-v11",
      center: [-50, 20],
      zoom: 3,
    });

    function generatePoints(count) {
      const features = [];
      for (let i = 0; i < count; i++) {
        const lon = -80 + Math.random() * 60;
        const lat = 0 + Math.random() * 50;
        const intensity = Math.random();
        const pointType =
          intensity > 0.6 ? "pollution" : intensity > 0.3 ? "temp" : "normal";

        features.push({
          type: "Feature",
          properties: {
            title: `Point ${i}`,
            pointType,
            description: `Type: ${pointType}, Intensité: ${(
              intensity * 100
            ).toFixed(1)}%`,
          },
          geometry: { type: "Point", coordinates: [lon, lat] },
        });
      }
      return { type: "FeatureCollection", features };
    }

    const geojsonData = generatePoints(100);

    map.on("load", () => {
      map.addSource("points", {
        type: "geojson",
        data: geojsonData,
        cluster: true,
        clusterRadius: 50,
      });

      // Clusters
      map.addLayer({
        id: "clusters",
        type: "circle",
        source: "points",
        filter: ["has", "point_count"],
        paint: {
          "circle-color": "#51bbd6",
          "circle-radius": [
            "step",
            ["get", "point_count"],
            20,
            100,
            30,
            750,
            40,
          ],
        },
      });

      // Cluster Count
      map.addLayer({
        id: "cluster-count",
        type: "symbol",
        source: "points",
        filter: ["has", "point_count"],
        layout: {
          "text-field": "{point_count_abbreviated}",
          "text-size": 12,
        },
        paint: {
          "text-color": "#fff",
        },
      });

      // Unclustered Points
      map.addLayer({
        id: "unclustered-point",
        type: "circle",
        source: "points",
        filter: ["!", ["has", "point_count"]],
        paint: {
          "circle-color": [
            "match",
            ["get", "pointType"],
            "pollution",
            "#FF0000",
            "temp",
            "#0000FF",
            "#00FF00",
          ],
          "circle-radius": 8,
        },
      });

      let popupOpened = null;
      let resetTimeout = null;

      map.on("click", "unclustered-point", (e) => {
        const features = e.features[0];
        const coords = features.geometry.coordinates.slice();
        const { title, description } = features.properties;

        if (popupOpened) {
          // Si un popup est déjà ouvert, c'est considéré comme un double-clic
          clearTimeout(resetTimeout); // Empêche la remise à zéro si le double-clic est détecté
          popupOpened.remove();
          popupOpened = null;
          this.goToDashboard(); // Redirige vers le dashboard
        } else {
          // Sinon, on ouvre un nouveau popup
          popupOpened = new mapboxgl.Popup()
            .setLngLat(coords)
            .setHTML(
              `
          <strong>${title}</strong><br>
          ${description}<br>
          <em style="color: grey;">Tap or Click again on this point for more details</em>
      `
            )
            .addTo(map);

          // Démarrer un timer de 1 seconde pour réinitialiser popupOpened si pas de double-clic
          resetTimeout = setTimeout(() => {
            popupOpened = null;
          }, 2000);
        }
      });

      // Curseur en mode pointer sur les points non clusterisés
      map.on("mouseenter", "unclustered-point", () => {
        map.getCanvas().style.cursor = "pointer";
      });

      map.on("mouseleave", "unclustered-point", () => {
        map.getCanvas().style.cursor = "";
      });

      // Zoom sur cluster au clic
      map.on("click", "clusters", (e) => {
        const features = map.queryRenderedFeatures(e.point, {
          layers: ["clusters"],
        });
        const clusterId = features[0].properties.cluster_id;
        map
          .getSource("points")
          .getClusterExpansionZoom(clusterId, (err, zoom) => {
            if (err) return;
            map.easeTo({
              center: features[0].geometry.coordinates,
              zoom: zoom,
            });
          });
      });

      map.on("mouseenter", "clusters", () => {
        map.getCanvas().style.cursor = "pointer";
      });

      map.on("mouseleave", "clusters", () => {
        map.getCanvas().style.cursor = "";
      });
    });
  },
  methods: {
    goToDashboard() {
      this.$router.push("/dashboard");
    },
  },
};
</script>

<style>
#map {
  height: 100vh;
}

.legend-control {
  position: absolute;
  bottom: 20px;
  right: 10px;
  background: white;
  padding: 10px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.legend-item {
  display: flex;
  align-items: center;
  margin: 4px 0;
}

.legend-color {
  width: 16px;
  height: 16px;
  margin-right: 8px;
  border-radius: 2px;
}
</style>
