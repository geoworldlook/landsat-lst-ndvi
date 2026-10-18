# Land surface temperature and NDVI from Landsat 8

A short ArcPy script that derives **land surface temperature (LST)** and **NDVI** from one Landsat 8 Collection 2
Level-1 scene (WRS-2 path 191, row 22, 15 August 2020). Written in 2024 as a GIS exercise; kept as an example of
working with thermal data and radiometric metadata.

## Method

1. Read bands 4 (red), 5 (NIR) and 10 (thermal IR) and the scene metadata (`MTL.txt`).
2. Convert band 10 digital numbers to top-of-atmosphere radiance with the `RADIANCE_MULT/ADD` coefficients.
3. Convert radiance to brightness temperature with the thermal constants `K1` and `K2`.
4. Compute NDVI = (NIR − Red) / (NIR + Red).
5. Estimate the vegetation proportion and land surface emissivity: ε = 0.004 · Pv + 0.986.
6. Apply the single-channel emissivity correction to obtain LST, then save it as a 30 m GeoTIFF.

## Run

Requirements: ArcGIS Pro with `arcpy`, `numpy` and `matplotlib`.
Input paths are hard-coded at the top of [`lst_ndvi_landsat8.py`](lst_ndvi_landsat8.py); change them to your scene first.

```bash
python lst_ndvi_landsat8.py
```

## Known limitations

- Brightness temperature is converted to °C *before* the emissivity correction, which expects kelvin.
  Absolute LST values are therefore biased, although relative spatial patterns are preserved.
- The vegetation proportion uses the square root of scaled NDVI instead of the more common square (Carlson & Ripley 1997).
- There is no atmospheric correction (single scene, single channel).

**Status:** 2024, not maintained.

## License

[MIT](LICENSE)
