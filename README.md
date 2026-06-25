[README_GLM_GOES_19_Downloader.md](https://github.com/user-attachments/files/29341158/README_GLM_GOES_19_Downloader.md)
# GLM GOES 19 Downloader

Python downloader for GLM lightning-detection NetCDF files from the public NOAA GOES S3 archive.

The repository name references GOES-19, but the current script is configured to download **GOES-16** GLM Level 2 LCFA files from:

```text
s3://noaa-goes16/GLM-L2-LCFA/
```

The sample file included in the repository also follows the GOES-16 naming convention:

```text
OR_GLM-L2-LCFA_G16_s20240010000000_e20240010000200_c20240010000213.nc
```

## Objective

Download GLM lightning activity files in NetCDF format for a selected day and year.

The current script:

- Connects anonymously to the public NOAA GOES-16 S3 bucket.
- Lists GLM Level 2 LCFA files.
- Uses a hardcoded date: January 1, 2024.
- Iterates through all 24 hours of the day.
- Downloads 180 GLM scan files per hour.
- Saves each file using its original filename.

## Repository Structure

```text
GLM_GOES_19_Downloader/
└── GLM/
    ├── Descarga_Lig_Det.py
    └── Archivos/
        └── OR_GLM-L2-LCFA_G16_s20240010000000_e20240010000200_c20240010000213.nc
```

## Data Source

The downloader uses the public NOAA GOES-16 S3 bucket:

```text
s3://noaa-goes16/GLM-L2-LCFA/YYYY/DDD/HH/
```

Where:

- `YYYY` is the year.
- `DDD` is the Julian day of year.
- `HH` is the UTC hour.

For the current script:

```text
YYYY = 2024
DDD = 001
HH = 00 through 23
```

## Product

The script downloads:

```text
GLM-L2-LCFA
```

This is the Geostationary Lightning Mapper Level 2 Lightning Cluster-Filter Algorithm product. The files are distributed in NetCDF format and contain lightning detection information.

## Technologies Used

- Python 3
- numpy
- netCDF4
- s3fs

## Installation

Clone the repository:

```bash
git clone https://github.com/fabriciolopretto/GLM_GOES_19_Downloader.git
cd GLM_GOES_19_Downloader/GLM
```

Install dependencies:

```bash
pip install numpy netCDF4 s3fs
```

## Usage

Run:

```bash
python Descarga_Lig_Det.py
```

The script will download files for January 1, 2024 from the GOES-16 GLM LCFA archive.

## Configuration

To change the download date, edit these variables in `Descarga_Lig_Det.py`:

```python
dia_tex = '001'
anno_tex = '2024'
```

`dia_tex` uses Julian day format, from `001` to `365` or `366` depending on the year.

To adapt the script to GOES-19, the S3 bucket and product path would need to be updated when the corresponding public archive path is available.

## Notes

The current script saves downloaded files in the working directory because it calls:

```python
fs.get(files[j], files[j].split('/')[-1])
```

If you want files saved into `GLM/Archivos/`, update the destination path in the `fs.get()` call.

The script assumes each hour contains 180 files. If the archive has fewer files for a given hour, the loop may fail. A more robust version would iterate over `len(files)` instead of a fixed range.

