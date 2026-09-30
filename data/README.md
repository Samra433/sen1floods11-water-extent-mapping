
## Dataset
**Sen1Floods11** hand-labeled flood-event chips: https://github.com/cloudtostreet/Sen1Floods11

## Download
Install the Google Cloud SDK, then run from the repository root:

```bash
mkdir -p data/S1Hand data/S2Hand data/LabelHand data/splits
gsutil -m cp -r gs://sen1floods11/v1.1/data/flood_events/HandLabeled/S1Hand/*    data/S1Hand/
gsutil -m cp -r gs://sen1floods11/v1.1/data/flood_events/HandLabeled/S2Hand/*    data/S2Hand/
gsutil -m cp -r gs://sen1floods11/v1.1/data/flood_events/HandLabeled/LabelHand/* data/LabelHand/
gsutil -m cp -r gs://sen1floods11/v1.1/splits/flood_handlabeled/*                data/splits/
```

## Expected folders
- `S1Hand/`: Sentinel-1 VV/VH chips 
- `S2Hand/`: Sentinel-2 13-band chips 
- `LabelHand/`: water labels 
- `splits/`: train, valid, test and Bolivia split lists
