## Building CHaiDNN in SDx 2018.3 on Windows 10 for Zynq UltraScale+ MPSoC ZCU104 using the original repository

1. Launch the **SDx development environment** using the desktop icon or the Start menu.
   The **Workspace Launcher** dialog appears

2. Click **Browse** to enter a workspace folder used to store your projects (you can use workspace folders to organize your work), then click **OK** to dismiss the **Workspace Launcher** dialog.
    The SDx development environment window opens with the **Welcome** tab visible when you create a new workspace. The Welcome tab can be closed by clicking the **X** icon or minimized if you do not wish to use it.

3. Select **File → New → SDx Application Project** from the SDx development environment menu bar.
    The **New SDx Project** dialog box opens.

4. Specify the name of the project. For example `CHaiDNN`.

5. Click **Next**.
   The **Choose Hardware platform** page appears.

6. From the **Choose Hardware platform** page, select `zcu104`. Click **Next**. To use a custom platform, click on **Add Custom Platform** and give the path to custom platform and click **OK**.
   The **System Configuration** page appears.

7. Use default settings and click **Next**.

8. Select **Empty Application** and click **Finish**.

9. Download CHaiDNN project from the Xilinx repository. <a href="https://github.com/Xilinx/CHaiDNN">Link</a>. Beware of changes! When this guide was written the last commit to the master branch was ad3be78 from Oct 23, 2018.

10. From **Project Explorer** Pane, Right Click on the `src` directory under the created project and click **Import**.
   The **Import wizard** appears.

11. Expand the `General` option and select **File System**. Click **Next**.

12. Browse & Select the local copy of `CHaiDNN` Git repository. Click **OK**.

13. Expand `CHaiDNN` and Select the `software` and the `design` directories. Click **Finish**.

14. All the source files needed to build `CHaiDNN`, can be seen under `src` directory.

    >**:pushpin: NOTE:**  The `software/example` folder contains example files. Using SDx GUI, only 1 of them can be built at once. Exclude other example files from the project except the one you wish to build.  

15. Right click on project and select **C/C++ Build Settings**. In the **Configuration** drop down menu, Select **Release** as an active configuration.

16. In `SDS++ Compiler` add the following in the `command`
    ```
    sds++ -sds-hw XiConvolutionTop  <path to design>/conv/src/xi_convolution_top.cpp -clkid 1 -hls-tcl  <path to design>/conv/scripts/config_core.tcl -sds-end

    ```
    >**:pushpin: NOTE:**   `<path to design>` is the path to the project's `src/design` folder.

    >**:pushpin: NOTE:**  If custom platform is used as described in the document ["CUSTOM PLATFORM GENERATION"](CUSTOM_PLATFORM_GEN.md), then `clkid` must be set to 1.

17. In `Symbols`, add the following defined symbols
    ```
    __SDSOC;__CONV_ENABLE__;__DSP48E2__
    ```
18. Add the following undefined symbols
    ```
    __SYNTHESIS__;__POOL_ENABLE__;__DECONV_ENABLE__
    ```
19. In `Optimization` set the Optimization Level to None (-O0).

20. In `Directories`, add below paths
    ```
    <path to libraries>/opencv/arm64/include
    <path to libraries>/protobuf/arm64/include
    <path to libraries>/cblas/arm64/include
    <path to project>/src/design/conv/include
    <path to project>/src/design/conv/src
    <path to project>/src/design/pool/include
    <path to project>/src/design/pool/src
    <path to project>/src/design/deconv/include
    <path to project>/src/design/deconv/src
    ```
    >**:pushpin: NOTE:**  `<path to libraries>` is the path to `SD_Card` directory in local `CHaiDNN` repository. `<path to project>` is the path to SDx project.

21. In `SDS++ Linker` add the following in the `command`
    ```
    sds++ -xp param:compiler.skipTimingCheckAndFrequencyScaling=1 -xp "vivado_prop:run.impl_1.{STEPS.OPT_DESIGN.ARGS.MORE OPTIONS}={-directive Explore}"  -xp "vivado_prop:run.impl_1.{STEPS.PLACE_DESIGN.ARGS.MORE OPTIONS}={-directive Explore}" -xp "vivado_prop:run.impl_1.STEPS.PHYS_OPT_DESIGN.IS_ENABLED=1" -xp "vivado_prop:run.impl_1.{STEPS.PHYS_OPT_DESIGN.ARGS.MORE OPTIONS}={-directive Explore}" -xp "vivado_prop:run.impl_1.{STEPS.ROUTE_DESIGN.ARGS.MORE OPTIONS}={-directive Explore}" -xp "vivado_prop:run.synth_1.{STEPS.SYNTH_DESIGN.TCL.PRE}={<path to design>conv/scripts/mcps.tcl}" -xp "vivado_prop:run.impl_1.{STEPS.PLACE_DESIGN.TCL.PRE}={<path to design>/conv/scripts/mcps.tcl}" -Wno-unused-label
    ```
    >**:pushpin: NOTE:**   `<path to design>` is the path to the project's `src/design` folder.
    
 22. In `SDS++ Linker`, Select `Libraries` and add the following libs
     ```
     opencv_core;lzma;tiff;png16;z;jpeg;opencv_imgproc;opencv_imgcodecs;dl;rt;webp;protobuf;openblas
     ```

 23. Add library paths
     ```   
     <path to libraries>/opencv/arm64/lib
     <path to libraries>/protobuf/arm64/lib
     <path to libraries>/cblas/arm64/lib
     ```
     >**:pushpin: NOTE:**  **Note:** `<path to libraries>` is the path to `SD_Card` directory in local `CHaiDNN` repository.

24. Save and apply changes. Close the settings window.

25. Navigate to `<path to project>/src/software/init` folder in `Project Explorer` Pane. Right click on `xi_init.cpp` and open the properties.
    In Optimization, change the `Optimization Level` to None (-O0).

26. Apply changes and close the window.

27. Open `<path to project>/src/design/scripts/mcps.tcl` file and modify the path of `xdc` file as shown below.
    ```
    read_xdc <path to project>/src/design/conv/scripts/mcp_const.xdc
    ```
    >**:pushpin: NOTE:**   `<path to project>` is the path to SDx project.

28. Open `project.sdx` file, change the `Data motion network clock frequency (MHz)` to 100.00.

29. Open `<path to project>/src/design/conv/include/xi_conv_config.h` file and modify `lines 28-30, 43` as shown below.
     ```   
     #define XI_WTS_URAM_EN   1
     #define XI_ISTG_URAM_EN  1
     #define XI_OSTG_URAM_EN  1
     
     #define XI_PIX_PROC      16
     ```
     
30. Open `<path to project>/src/design/conv/src/xi_convolution_top.cpp` file and modify `lines 17, 18` as shown below.
     ```   
     #include "../include/xi_conv_config.h"
     #include "../include/xi_conv_desc.h"
     ```
     
31. Open `<path to project>/src/design/deconv/src/xi_deconv_top.cpp` file and modify `line 19` as shown below.
     ```   
     #include "../include/xi_deconv_config.h"
     ```
     
32. Open `<path to project>/src/design/pool/src/pooling_layer_dp_2xio_top.cpp` file and add include after `line 18` as shown below.
     ```   
     #include <stdio.h>
     ```    
     
33. Open `<path to project>/src/design/wrapper/dnn_wrapper.cpp` file and add modify `line 18` as shown below.
     ```   
     #include "../conv/include/xi_conv_config.h"
     ```    

34. Open `<path to project>/src/software/include/hw_settings.h` file and add modify `line 40` as shown below.
     ```   
     #define XI_PIX_PROC       	16
     ```
     
35. Open `<path to project>/src/software/swkernels/xi_eltwiswadd_top.cpp` file and add modify `line 38` as shown below.
     ```   
     #ifndef __SDSOC
     ``` 
36. Build the project.
