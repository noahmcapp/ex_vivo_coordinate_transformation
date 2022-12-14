# Ex Vivo MRI Coordinate Transformation

This code is capable of moving between T2 and FLASH coordinate systems, using the the original .nii.gz image files that the coordinates were accessed.

## Getting Started

These instructions will get you all necessary code and binaries to run the coordinate transformations.

### Prerequisites

- bash or sh
- Python 3.X
    - Numpy
        - python3 -m pip install numpy
    - Pandas
        - python3 -m pip install pandas
    - SimpleITK
        - python3 -m pip install simpleitk
- c3d_affine_tool
    - Should be installed with ITK-SNAP

### Installing

Download/Clone this repository to a secure location

    git clone https://github.com/noahmcapp/ex_vivo_coordinate_transformation.git ~/ex_vivo_coordinate_transformation

Add the cloned directory to your [PATH environment variable](https://www.java.com/en/download/help/path.html)

    PATH=$PATH:~/ex_vivo_coordinate_transformation/bin

Run ex_vivo_convert_t2 to ensure everything worked

    $ ex_vivo_coordinates % ex_vivo_convert_t2
    usage: ex_vivo_convert_t2 --coordinates /path/to/coords.csv --t2 /path/to/t2.nii.gz --flash /path/to/flash.nii.gz

## Running the programs

### Input CSV format

[Template CSV File](https://github.com/noahmcapp/ex_vivo_coordinate_transformation/blob/main/etc/coordinates_template.csv)

A single CSV file represents a list of coordinates from a patient. The coordinates may be named, and are represented by their FLASH version, T2 version, as well as the slab that the coordinate falls in.

### T2 to FLASH

This will add the FLASH coordinates to your input CSV file

#### Inputs
- patientX_coordinates.csv
    - Filled w/ T2_x,T2_y,T2_z data
- patientX_t2.nii.gz
    - Image where the coordinates were retrieved
- patientX_flash.nii.gz
    - Image where the coordinates are transforming to
    
#### Example
    ex_vivo_convert_t2 -c /path/to/coordinates.csv -t2 /path/to/t2.nii.gz -flash /path/to/flash.nii.gz

### FLASH to T2

This will add the T2 coordinates to your input CSV file

#### Inputs
- patientX_coordinates.csv
    - Filled w/ FLASH_x,FLASH_y,FLASH_z data
- patientX_flash.nii.gz
    - Image where the coordinates were retrieved
- patientX_t2.nii.gz
    - Image where the coordinates are transforming to

#### Example
    ex_vivo_convert_flash -c /path/to/coordinates.csv -flash /path/to/flash.nii.gz -t2 /path/to/t2.nii.gz 

### Slab Number Generation

This will add the slab numbers to your input CSV file

#### Inputs
- patientX_coordinates.csv
    - Filled w/ T2_x,T2_y,T2_z data
- /path/to/brainmolds/
    - location where the reslice and slab masks are stored. NOTE: On Box this would be Box/Hemisphere_segmentation/subjects
- INDD000000X
    - INDD ID of the patient where the coordinates are from
    
#### Example
    ex_vivo_get_slab --indd INDD123456R --directory $BOX/Hemisphere_segmentation/subjects --coordinates /path/to/coordinates.csv

### Additional Files
- flash_to_t2_mask.mat
    - transformation matrix provided to Sandy
- t2_to_flash_mask.mat
    - inverse of above transformation matrix
- greedy
    - a binary of [Greedy](https://greedy.readthedocs.io/en/latest/) provided by Pulkit that works with this code
    
### Sample Outputs

![Input CSV](https://github.com/noahmcapp/ex_vivo_coordinate_transformation/blob/main/etc/before.png)

![Output CSV](https://github.com/noahmcapp/ex_vivo_coordinate_transformation/blob/main/etc/after.png)
