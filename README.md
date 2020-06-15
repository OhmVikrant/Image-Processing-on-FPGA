<img src = 'https://github.com/OhmVikrant/Image-Processing-on-FPGA/blob/master/Realtime_IP/hardware/img/de2i.jpg' style="width:400px; float: center; margin: 0 40px 40px 40px;"></img>
# Image-Processing-on-FPGA
![purple-divider](https://user-images.githubusercontent.com/7065401/52071927-c1cd7100-2562-11e9-908a-dde91ba14e59.png)


The  project  operates  on  real‐time  video  and  performs  image  processing all using the [de2i-150 FPGA Development Kit from Terasic Inc](https://www.intel.com/content/www/us/en/programmable/solutions/partners/partner-profile/terasic-inc-/board/de2i-150-fpga-development-kit.html). What it does is that it bolds  the  edges,  and  changes  the  color  components  in  each  frame input.  It  smoothens  each  frame  and  makes  it  brighter  and  blurred.  Overall,  it  modifies  the  image  to  make  it  look  like  a  painting  rather  than  a  normal  photograph.  An  interesting  challenge  involved  in  this  case  is  to  provide  processed  frames  on‐the‐fly  from  a  real‐time  video  feed.  The  output  is  displayed on the  monitor  through  VGA.

## Description of the File system:
![green-divider](https://user-images.githubusercontent.com/7065401/52071924-c003ad80-2562-11e9-8297-1c6595f8a7ff.png)


RealTimeIP.sv --> top-level module, connecting all the blocks in the system.

RealTimeIP.qpf --> Quartus Project file. Use this to open the Project in Quartus

RealTimeIP.qsf --> The pin mapping/assignment file generated in Quartus 

stp3.stp    --> SignalTap file used for debugging purpose

### Camera interface and SDram Control:
v/CCD_Capture.v --> Camera interface module, taking input from camera. (provided by Terasic)

v/RAW2RGB.v --> This module accepts raw data from CCD_Capture and converts into RGB format. (provided by Terasic)

Sdram_Control/* --> The SDRAM interface. The pixels from camera are written in SDRAM each time using the Frame Read buffer.
Simultaneously, they are read from it using the Frame Write buffer. (provided by Terasic)


### Custom Block:
v/LineBuffer1.v --> This is the 3-line buffer which stores 3 lines of camera data, shifting in and out the pixels eaxh time. This outputs the 3x3 grid of pixels needed for edge detection.

intensityCalc.v --> This module calculates the intensity of each pixel coming out of the LineBuffer module

edgedetectH.v --> horizontal edge detector module, detects any horizontal edge if present in the grid output from LineBuffer

edgedetectV.v --> vertical edge detector module, detects any vertical edges

averager.v --> averager module, which takes in the main pixel of interest each time, averages it with the value stored in SDRAM previously (time averaging)

ColorSimplify.v --> Color reduction module

VGA_Controller.v --> The is the VGA Controller module provided by Terasic. The top level module shows the inputs to this module, coming from a MUX.
