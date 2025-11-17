# Lineage 2 Geodata Collection

All assets shared in this repository belong to their respective owners. You may launch the Lineage 2 server for educational purposes only.

© 2024, [Geekrainian](https://geekrainian.com/?utm_source=gitlab&utm_medium=geekrainian&utm_campaign=lineage2-geodata)

## How to install geodata

1 - Open data folder `gameserver\data`
2 - Copy contents folder geodata in `gameserver\data\geodata`
3 - Copy contents folder pathonde in `gameserver\data\pathnode`
4 - To use the Geodata you must go on `gameserver/config/head/geodata.properties`

SET `GeoData = `

0 = geodata disabled (default).
1 = enabled geodata.

If `ForceGeodata = true`, it requires ~ 3 GB
If `ForceGeodata = false`, then the required rate of screws 7200

2 = geodata and patchnod (search path) are included.

If `ForceGeodata, ForcePathNod = true`, it requires about 5 GB
If `ForceGeodata, ForcePathNod = false`, then the required rate of screws 7200

-1 = Patchnod (search path) enabled.

If `ForcePathNod = true`, it requires about 2 GB
If `ForcePathNod = false`, then the required rate of screws 7200

Test function! There may be mistakes!

## How to create your own geodata

Tools needed:

- [Stazis L2 Geo Converter](http://www.mediafire.com/file/irzey5hk0tyywub/GeoConv_v93b.zip)
- [G16ed](http://www.mediafire.com/file/2v2288vlbq6n2bk/G16ed.zip) - a heightmap editor that works directly with the UEd3 G16 format (16-bit greyscale)
- [UTPackage](http://www.mediafire.com/file/vuddpqsd5ik9g3c/UTPackage.zip) - to extract game resources
- [Unreal Engine 2 Editor](http://www.mediafire.com/file/yx53pt73a5e6yk3/UE2Runtime-22261903.zip) - to create UTX packages
- [L2J-GeoEditor](http://www.mediafire.com/file/1j5lhll1mn1uy2x/l2j-GeoEditor-v17.zip) - to convert to L2J format and create the PathNode
- HEX Editor (UEStudio or Ultraedit)

Important: In this example I will use the map T_22_19, but applies the same procedure for any map.

Important: If you use a Classic map remove \_Classic from both Textures and MAP file names.

First we need to extract heightmap images from UTX files (T_22_19.UTX, T_22_20.UTX etc.)

- Put this file T_22_19.UTX in UTPackage/Textures and execute "unpack.bat"
- Open UTPackage/RAW folder and find the file 22_19.raw
- Open this file with Ultraedit and search (Ctrl + f) for "40 80 10", the first byte after this string is the start of the heightmap image.
- Copy this address, in this case is 107h
- Open the windows calculator and switch to "Scientific" mode, select "Hex" and input "107", select "Dec" and now you have "263".
- Open G16ed, go to "File -> Import -> RAW data, search for 22_19.raw and in the field "Data start offset" input "263" and click in Input, Ok, Ok.
- Go to "File -> Save (Ctrl + S)" and use the name of the map for your new image (22_19.BMP). Now you have a perfect G16 heightmap image.

Now we need to create an UTX file with the image that we have saved.

- Open UnrealEd and in the windows "Textures" go to "File -> New" and complete the fields.
  Info -> Package: T_22_19, Group: Height, Name: 22_19, Class: Raw Material
  Properties -> MaterialClass: Class"Engine.Texture" (Select Texture from drop-down)
- Go to "File -> Import" and select the image 22_19.BMP and ensure that the fields are correct.
  Info -> Package: T_22_19, Group: Height, Name: 22_19
  Options -> Masked: uncheck, Generate MipMaps: uncheck, Detail Hack?: uncheck, Compression: none
- Go to "File -> Save" and save this file as T_22_19.utx.
  Now we can create our Geodata using Stazis L2 Geo Converter (GeoConv)
- Navigate to the Textures folder in the game, rename your original T_22_19.utx to T_22_19_O.utx and put in this folder our new T_22_19.utx
- Open GeoConv and change this params.
  Min Plane Angle to XY: 20, Stairs Height: 10, Optimization Different: 80
- Click "Open Packages" and select 22_19.unr in "Lineage II/MAPS", allow the process to finish and now you have your GEO 22_19_conv.dat in the folder "Lineage II/MAPS".
- If you get an error here, change your system's decimal symbol to . from Control Panel -> Regional Settings.
- Convert this GEO to L2J format and create the PathNode with L2j GeoEditor or HDGE.

Known Issues:

- Some geodata are not correctly generated or can't be generated.
- Using "Stairs Height: 10" can cause problems with the stairs (If you use "8" check all the stairs in the map for correct NSEW).

Further manual adjustments can be made using [G3DEditor](http://www.mediafire.com/file/ps2d2kc0hwc2d54/G3DEditor_win_x64_20110920.zip).
