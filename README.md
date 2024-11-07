# Docker AGL builder

Docker files to build [Automotive Grade Linux](https://www.automotivelinux.org/) demo images.

## Building instructions

To create the Docker image, inside the docker folder, run:

```
docker build -t agl-builder .
```

To prepare the build, create a build directory with 2 sub-directories:

- `inputdata`: it will be used for optional input files, e.g. Renesas proprietary drivers (see note below).
- `outputdata`: this is the directory where the docker will save the output.

These sub-directories should be mounted as volumes when running docker container.

The following variables control AGL parameters like version, output target and machine, and can be set as environment variables in Docker. Please refer to [AGL documentation](https://docs.automotivelinux.org/en/pike/#) for details.

- *AGL_BRANCH* The AGL version code name (default = "quillback").
- *AGL_MACHINE* The AGL target machine (default = "ebisu", which is Renesas R-Car Evaluation Board)
- *AGL_FEATURE* AGL feature(s) to be loaded for build (default = "agl-demo")
- *AGL_RECIPE* AGL image recipe to be built (default = "agl-ivi-demo-qt")

Please also override *git* user and email variables.

- *GIT_USER*
- *GIT_EMAIL*

#### Note about Renesas R-Car boards

One of AGL officially supported hardware is [Renesas R-Car](https://www.renesas.com/en/products/automotive-products/automotive-system-chips-socs) SoC family. To build for some boards, proprietary drivers protected by NDA are necessary. Some boards instead only require to accept the license. Please refer to this [AGL documentation page](https://docs.automotivelinux.org/en/pike/#01_Getting_Started/02_Building_AGL_Image/09_Building_for_Supported_Renesas_Boards/#11-downloading-proprietary-drivers) for details on how to get them. In both cases, place the driver package files in the `inputdata` folder to build porperly.
