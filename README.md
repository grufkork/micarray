# 16-channel Microphone Array for Spatial Audio Recording
This is an inexpensive 16-channel MEMS microphone array for recording spatial audio made in KiCad 8.0. It can  be used for creating immersive audio/video content, locating mechanical faults, tracking moving objects in a room and more.

This design is developed for the [Build-Your-Own Ambisonic Microphone Arrays](https://github.com/AppliedAcousticsChalmers/ambisonic-microphone-arrays) project, where you can also find more information regarding processing of the resulting recordings.

The array consists of four identical boards connected in a circle, allowing for low manufacturing costs (<100$ total, assembly included). These are then placed around an acoustically occluding sphere, emulating a persons head and allowing for generating realistic spatial audio. 

![](https://github.com/grufkork/micarray/blob/8941890ad8e700dc878425aede164a82e50adaf4/imgs/16ch_sphere.jpg)

![](https://github.com/grufkork/micarray/blob/8941890ad8e700dc878425aede164a82e50adaf4/imgs/16ch.jpg)

The array is connected to the [MiniDSP MCHStreamer Lite](https://www.minidsp.com/products/usb-audio-interface/mchstreamer-lite) sound card which receives and digitises the 8x PDM output (the non-lite version should work too, but has not been tested). 


### File structure

| Directory           | Contents                                                                                                      |
| ------------------- | ------------------------------------------------------------------------------------------------------------- |
| micarray            | 16ch microphone array, main design. Contains KiCAD project files and some exported models and visualisations. |
| imgs                | Images for this document                                                                                      |
| micarray_8ch_camera | Unfinished 8ch version for a 360° camera.                                                                     |
| 3dcammodel          | 3D-scan of 360° camera and visualisations                                                                     |

### Ordering
Production files are generated using [JLC Fabrication Toolkit](https://github.com/bennymeg/Fabrication-Toolkit) plugin for KiCad 8.0. All parts have `LCSC Part #`-fields so they can be assembled by JLC, but I would still recommend double checking everything, in particular small & cheap components so they have not gotten low availability. The full array consists of 4 quarter pieces connected in a circle. JLC minimum order size is 5, but having an extra module at hand has proven to be useful as all our orders have always had one mic with worsened performance. Whether this is due to JLC:s manufacturing process, errors in manufacturing, damages during transport or just bad luck is not known.

### Assembly
#### Shortening the inter-board headers
*This part is not mandatory for basic function, but if you want a precise, snug fit with exact distances between each microphone it is required. There will otherwise be a small gap between each board. **

Due to the tight space constraints, the headers connecting the boards to each other need to be trimmed. First remove the plastic holding the male header pins together, preferably by locally applying heat with a hot air gun and with pliers gently but decisively sliding the softened plastic off along the pins. Using a soldering iron or similar to try to melt plastic off is not recommended, as the residual plastic will fuse with the pins and form a thin insulating surface, which then needs to be sanded off. Be carful trying to cut the plastic into smaller parts, as this might bend the pins.

You can then try connecting the boards, noting how large the gap between them is (should be a couple of millimeters). Disconnect the boards and clip off the male pins so that the boards connect flush, edge-to-edge.

#### Connecting to the MCHStreamer
On each of the boards there is a vertical 2mm 12-pin header. This is to be connected to J3 on the MCHStreamer (check page 2 of [the datasheet](https://www.minidsp.com/images/documents/Product%20Brief-MCHStreamer%20Lite.pdf)). Note how the pins are mirrored, so they can be connected "towards" each other. As such, a 2x6 pin cable can be used. Which header is used only affects which mic is #1. Specifically, the first mic on the board opposite the connector will be channel 1. 

![](https://github.com/grufkork/micarray/blob/78456a09cc365f2688d070a698d67fb4b68df57a/imgs/connecting.png)

To interface with the array, upload the 16ch PDM firmware to the MCHStreamer. 

### Technical & Mechanical Details
The 16 mics are evenly spaced in a circle with 60.743mm radius. The array is made to fit on a 60mm sphere. The M3 mounting holes are 71.692mm out, 3.2mm in diameter, 27.1° counter-clockwise relative board edge.

The mics used are the Infineon IM73D122V01. They appear to currently be the best-performing MEMS mics available, using a novel double-membrane technology. Frustratingly, these have a slightly larger footprint with a different pad order than most other similar MEMS mics on the market, so be wary if swapping them for a different model.

The `stable` branch is the main, cleaned-up version of the old development `shrinkening` branch. 

### Troubleshooting
The primary problem you may experience is massive amounts of noise on one or several channels. This appears to be caused by the clock signal from the MCHStreamer ringing (overshooting) strongly. To prevent this, a 150pF capacitor is included in the design, connected between ground and the clock input. However, if you are still experiencing problems, soldering on some additional capacitance between `GND` and `PDM_CLK-IN` on the backside of one of the inter-board headers should resolve the issue. This fix is visible in the second image above, being the two yellow wires sticking out from one of the connectors.



## 8ch version
The 8ch version is an old, untested version of the 16ch for recording spatial audio with a 360° camera. It is not tested, but might work with some extra capacitance between `GND` and `PDM_CLK-IN`.
![8ch](https://github.com/grufkork/micarray/blob/617b07e39810f13811206140864eb196cf2ee92b/imgs/8ch.png)
