## Source

CSD's 2001 census map files.

## Derived

`dc_land.shp` is given by CSD. `dcca_land.shp` derived from `dcca.shp` plus `coast.shp` using QGIS a few years ago but we can not reproduce it now in the current version of QGIS.

GeoJSONs are derived as:

```bash
ogr2ogr -s_srs EPSG:2326 -t_srs EPSG:4326 -f geoJSON dc_land.geo.json dc_land.shp
ogr2ogr -s_srs EPSG:2326 -t_srs EPSG:4326 -f geoJSON dcca_land.geo.json dcca_land.shp
```

- EPSG:2326 is Hong Kong 1980 CRS. 
- EPSG:4326 is WGS 1984 CRS, which you can understand as the standard worldmap using normal longitude and latitude values and is centered at (0, 0).

The files are then simplified using a command-line tool in NodeJS, [mapshaper](https://github.com/mbloch/mapshaper/wiki/Command-Reference):

```bash
mapshaper dc_land.geo.json -simplify dp 5% -o format=geojson -o precision=0.001 dc_land.lowres.geo.json
mapshaper dcca_land.geo.json -simplify dp 5% -o format=geojson -o precision=0.001 dcca_land.lowres.geo.json
```

The low resolution version are generally good enough for web use. The resolution is 0.001, which is around the size of a court/ building/ complex. This is a tradeoff between loading time and resolution.

