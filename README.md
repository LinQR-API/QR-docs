# QR Code API  <!-- omit in toc -->

This document describes methods, parameters and usage of Fast QR Code Generator API

## Table of Contents <!-- omit in toc -->
- [API usage methods](#api-usage-methods)
  - [Simple QR Code generation](#simple-qr-code-generation)
  - [QR Code generation with image upload](#qr-code-generation-with-image-upload)
  - [QR Code generation with separately uploaded image](#qr-code-generation-with-separately-uploaded-image)
- [API parameters](#api-parameters)
  - [Basic parameters](#basic-parameters)
  - [Appearance parameters](#appearance-parameters)
  - [QR Code specific parameters](#qr-code-specific-parameters)
- [Styles' list](#styles-list)
- [Eyes styles' list](#eyes-styles-list)

## API usage methods

### Simple QR Code generation
![Simple QR Code generation](schemes/generate.svg)

### QR Code generation with image upload
![QR Code generation with image upload](schemes/image+generate.svg)
### QR Code generation with separately uploaded image
![QR Code generation with separately uploaded image](schemes/upload+generate.svg)

## API parameters

### Basic parameters
| Parameter | Type             | Default value           | Accepted values                 | Description                                                                                                                                                                                                                                                                                                   |
| --------- | ---------------- | ----------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| text      | `string`         | **required in request** | `string` of (almost) any length | Text which will be encoded in QR. Value have to be URL encoded. Maximum length which can be encoded in QR as UTF-8 with the lowest error correction level is 2953 bytes, resulting in 177x177 modules code. For more information please refer to: [QRcode.com](https://www.qrcode.com/en/about/version.html). |
| size      | (`float`\|`int`) | 150                     | <100;2500>                      | Default size (width and height) in pixels for generated image of QR Code. If requested value is `float` it will be casted to nearest `int`.                                                                                                                                                                   |
| format    | `string`         | `svg`                   | {`svg`, `png`, `pdf`, `jpg`}    | File format for generated QR.                                                                                                                                                                                                                                                                                 |


### Appearance parameters
| Parameter                                             | Type               | Default value | Accepted values                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------------------------------------- | ------------------ | ------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| style                                                 | `string`           | `default`     | **see [Styles' list](#styles-list)**           | Style applied to QR Code modules. Please refer to table below where all available styles are listed.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <a name="style_color">style_color</a>                 | `string`           | `#000000`     | <`#000000`; `#FFFFFF`>                         | Base color for QR Code modules referred as RGB triple value presented as hex number. Can be obtained from [w3schools.com](https://www.w3schools.com/colors/colors_picker.asp).                                                                                                                                                                                                                                                                                                                                       |
| inner_eye_style                                       | `string`           | `default`     | **see [Eyes styles' list](#eyes-styles-list)** | Style applied to inner QR Code eyes. Please refer to table below where all available styles are listed.                                                                                                                                                                                                                                                                                                                                                                                                              |
| <a name="inner_eye_color">inner_eye_color</a>         | `string`           | `None`        | <`#000000`; `#FFFFFF`>                         | Base color for inner QR Code eyes. If this value is not specified, base color is inherited from data modules color (see parameter: [style_color](#style_color)).                                                                                                                                                                                                                                                                                                                                                     |
| outer_eye_style                                       | `string`           | `default`     | **see [Eyes styles' list](#eyes-styles-list)** | Style applied to outer QR Code eyes. Please refer to table below where all available styles are listed.                                                                                                                                                                                                                                                                                                                                                                                                              |
| <a name="outer_eye_color">outer_eye_color</a>         | `string`           | `None`        | <`#000000`; `#FFFFFF`>                         | Base color for inner QR Code eyes. If this value is not specified, base color is inherited from data modules color (see parameter: [style_color](#style_color)).                                                                                                                                                                                                                                                                                                                                                     |
| bg_color                                              | `string`           | `#FFFFFF`     | <`#000000`; `#FFFFFF`>                         | Background color for QR Code. If value is not specified, background area is removed resulting in transparent (format `png` or `svg`) or white (format `pdf` or `jpg`) background depending on choosen file format.                                                                                                                                                                                                                                                                                                   |
| <a name="fill_style">fill_style</a>                   | `string`           | `solid`       | {`solid`, `linearGradient`, `radialGradient`}  | Type of whole QR Code fill style. If gradient-style fill is requested, fill color of modules changes fluently from [style_color](#style_color) to [gradient_stop_color](#gradient_stop_color). In case of `linearGradient` color changes along straight line and in case of `radialGradient` color changes along radius from center of QR. If [outer_eye_color](#outer_eye_color) or [inner_eye_color](#inner_eye_color) parameter are specified, they override gradient behaviour on according outer or inner eyes. |
| <a name="gradient_stop_color">gradient_stop_color</a> | `string`           | `#000000`     | <`#000000`; `#FFFFFF`>                         | Second (stop) color of gradient style fill (see parameter: [fill_style](#fill_style))                                                                                                                                                                                                                                                                                                                                                                                                                                |
| gradient_angle                                        | (`float`\|`int`)   | 0             | <0;360)                                        | Angle (in degrees) of linear gradient rotation measured clockwise. By default, horizontal gradient is applied. To reverse color direction use value of 180. Vertical gradients are indicated by values of 90 and 180 etc.                                                                                                                                                                                                                                                                                            |
| image                                                 | (`string`\|`file`) | `None`        | see [API Usage Methods](#api-usage-methods)    | Image file to be embedded into QR code                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| remove_background                                     | `boolean`          | `false`       | {`true`,`false`}                               |

### QR Code specific parameters

| Parameter  | Type   | Default value | Accepted values      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------- | ------ | ------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ec_level   | `char` | `M`           | {`L`, `M`, `Q`, `H`} | Error correction level for generated QR. Allows to restore some encoded data if it is partially unreadable. Literals correspond to amount of data which can be restored:<ul><li>`L`: approx. 7%</li><li>`M`: approx. 15%</li><li>`Q`: approx. 25%</li><li>`H`: approx. 30%</li></ul>Higher correction level causes in bigger resulting QR. **If image is embedded in QR, `H` correction level is choosen automatically and overrides user input.** For more information please refer to: [QRcode.com](https://www.qrcode.com/en/about/error_correction.html) |
| quiet_zone | `int`  | 4             | <0;4>                | Default size of margin with background color around generated QR Code. This length is expressed as number of modules of Code. Recommended value is 4, but most readers allow you to read the code with smaller or zero margin. For more information please refer to: [QRcode.com](https://www.qrcode.com/en/howto/code.html).                                                                                                                                                                                                                                |

## Styles' list

| Name         | Style example                                   |
| ------------ | ----------------------------------------------- |
| `default`    | ![Style: default](img/styles/default.png)       |
| `arrow`      | ![Style: arrow](img/styles/arrow.png)           |
| `circle`     | ![Style: circle](img/styles/circle.png)         |
| `classic`    | ![Style: classic](img/styles/classic.png)       |
| `heavyround` | ![Style: heavyround](img/styles/heavyround.png) |
| `lightround` | ![Style: lightround](img/styles/lightround.png) |
| `sieve`      | ![Style: sieve](img/styles/sieve.png)           |

## Eyes styles' list
| Name         | Inner eye                                           | Outer eye                                           |
| ------------ | --------------------------------------------------- | --------------------------------------------------- |
| `default`    | ![Style: default](img/eyes/default_inner.png)       | ![Style: default](img/eyes/default_outer.png)       |
| `circle`     | ![Style: circle](img/eyes/circle_inner.png)         | ![Style: circle](img/eyes/circle_outer.png)         |
| `cushion`    | ![Style: cushion](img/eyes/cushion_inner.png)       | ![Style: cushion](img/eyes/cushion_outer.png)       |
| `diamond`    | ![Style: diamond](img/eyes/diamond_inner.png)       | ![Style: diamond](img/eyes/diamond_outer.png)       |
| `dots`       | ![Style: dots](img/eyes/dots_inner.png)             | ![Style: dots](img/eyes/dots_outer.png)             |
| `heavyround` | ![Style: heavyround](img/eyes/heavyround_inner.png) | ![Style: heavyround](img/eyes/heavyround_outer.png) |
| `leaf`       | ![Style: leaf](img/eyes/leaf_inner.png)             | ![Style: leaf](img/eyes/leaf_outer.png)             |
| `left_eye`   | ![Style: left_eye](img/eyes/left_eye_inner.png)     | ![Style: left_eye](img/eyes/left_eye_outer.png)     |
| `right_eye`  | ![Style: right_eye](img/eyes/right_eye_inner.png)   | ![Style: right_eye](img/eyes/right_eye_outer.png)   |
| `shield`     | ![Style: shield](img/eyes/shield_inner.png)         | ![Style: shield](img/eyes/shield_outer.png)         |
| `sieve`      | ![Style: sieve](img/eyes/sieve_inner.png)           | ![Style: sieve](img/eyes/sieve_outer.png)           |
| `star`       | ![Style: star](img/eyes/star_inner.png)             | ![Style: star](img/eyes/star_outer.png)             |


**Acknowledgement:** The word *QR Code* is registered trademark of [DENSO WAVE INCORPORATED](http://www.denso-wave.com/en/en/) in Japan and other countries.