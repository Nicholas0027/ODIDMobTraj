<div align="center">

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Data Format](https://img.shields.io/badge/format-GeoJSON-blue.svg)
![Python](https://img.shields.io/badge/Python-3.x-blue.svg)
![Framework](https://img.shields.io/badge/Framework-GeoPandas-green.svg)

</div>

<div align="">

# Deciphering Delivery Mobility: A City-Scale, Path-Reconstructed Trajectory Dataset

</div>

> The rapid expansion of the on-demand economy has profoundly reshaped urban mobility and logistics, yet high-resolution trajectory data on delivery rider movements remains scarce. Here, we present a city-scale, high-resolution spatiotemporal trajectory dataset of on-demand delivery riders. This dataset was generated using a path-reconstruction methodology applied to an open dataset containing origin-destination (OD) delivery "wave" information. By simulating realistic cycling routes with a major commercial web map service, we effectively reconstructed detailed, continuous trajectories from sparse initial task data. This publicly available resource offers unprecedented opportunities for researchers in urban planning, transportation studies, logistics optimization, and computational social science.

<div align="center">

### ✨ [View the Interactive Data Visualization](https://odidmobtraj.netlify.app/) ✨

</div>

---

### 🚀 Key Contributions

1.  **A Novel Public Dataset:** We provide a new, city-scale, and high-resolution spatiotemporal trajectory dataset that captures the nuanced mobility of instant delivery riders, a resource previously unavailable to the public research community.
2.  **A Reproducible Framework:** We introduce a novel and reproducible methodological framework for reconstructing complex, multi-stop delivery tours from wave-based OD data, advancing beyond simple point-to-point simulations.
3.  **A New Research Paradigm:** This trajectory dataset offers a new paradigm for mobility and urban systems research where access to sensitive trajectory data is restricted, supporting studies on the gig economy, urban freight emissions, and logistics operations.

---

### 🔬 Methodology

Our path-reconstruction framework is designed to generate a continuous, high-resolution trajectory for each multi-stop delivery tour (called a "wave"). The process is as follows:

1.  **Decompose Delivery Waves:** Each delivery wave, representing a single rider's tour, is extracted from the source data.
2.  **Sequence Actions:** The "assign," "pickup," and "delivery" tasks within each wave are chronologically sorted to establish the precise sequence of locations a rider must visit.
3.  **Simulate Routes:** We systematically query the Amap routing API to generate a realistic cycling path between each consecutive point in the sequence.
4.  **Synthesize Trajectories:** The resulting path segments are concatenated to form a single, complete, and continuous spatiotemporal trajectory representing the rider’s entire tour.

---

### 📊 Technical Validation

The reconstructed dataset demonstrates high fidelity when compared to ground-truth metrics from the original data.

-   **Route Distance:** Excellent correlation (**Pearson's r = 0.92**) between reconstructed and real-world travel distances.
-   **Route Duration:** Strong correlation (**Pearson's r = 0.79**) between reconstructed and real-world travel times.

The dataset also reflects known urban delivery dynamics, including:
-   **Spatiotemporal Patterns:** Trajectory density aligns with Beijing's urban core and road network.
-   **Temporal Rhythms:** Clear bimodal peaks in delivery activity corresponding to lunch (11:00–13:00) and dinner (17:00–19:00) rushes.

---

### 💾 Dataset

The dataset is available on **Figshare** (you should add your specific DOI link here) and is provided in the **GeoJSON** format. It contains a `FeatureCollection` with two types of features:

1.  **Routes (`LineString`):** Complete, multi-stop delivery tours.
2.  **Action Points (`Point`):** Discrete stops (e.g., ASSIGN, PICKUP, DELIVERY) along a route.

#### Route Feature Schema
| Field Name  | Data Type | Description                                        |
| :---------- | :-------- | :------------------------------------------------- |
| `Route_id`  | Integer   | Unique identifier for each route.                  |
| `courier_id`| Integer   | Anonymized identifier for the delivery rider.      |
| `date`      | String    | Date of the route (YYYY-MM-DD).                    |
| `r_dis_all` | Float     | The total actual travel distance in meters.        |
| `nav_dis`   | Float     | The total reconstructed navigation distance in meters.|
| `r_dur_all` | Integer   | The total actual duration of the route in seconds. |
| `nav_dur`   | Integer   | The total reconstructed navigation duration in seconds. |
| `...`       | `...`     | *(see paper for full schema)* |

#### Action Point Feature Schema
| Field Name    | Data Type | Description                                   |
| :------------ | :-------- | :-------------------------------------------- |
| `Route_id`    | Integer   | Identifier of the parent route.               |
| `act_pt_id`   | String    | Unique identifier for the action point.       |
| `act_time`    | Integer   | UNIX timestamp for the action.                |
| `action_type` | String    | Type of action ('ASSIGN', 'PICKUP', 'DELIVERY'). |
| `...`         | `...`     | *(see paper for full schema)* |

---

### 👨‍💻 Usage Notes

The GeoJSON dataset can be easily loaded into most GIS software or data analysis libraries. For programmatic access, we recommend Python with the `geopandas` library.

```python
import geopandas as gpd

# Load the dataset
gdf = gpd.read_file("path/to/your/dataset.geojson")

# Separate routes and action points
routes = gdf[gdf['feature_type'] == 'route']
action_points = gdf[gdf['feature_type'] == 'action_point']

print(f"Loaded {len(routes)} routes and {len(action_points)} action points.")
