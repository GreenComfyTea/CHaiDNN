<table style="width:100%">
<tr>
<th width="100%" colspan="6"><img src="https://www.xilinx.com/content/dam/xilinx/imgs/press/media-kits/corporate/xilinx-logo.png" width="30%"/><h1>CHaiDNN-v2</h2>
</th>
</tr>
  <tr>
    <th rowspan="6" width="17%">Analysis and Eval</th>
   </tr>
<tr>
	<td align="center" colspan="2"><a href="./docs/SUPPORTED_LAYERS.md">Supported Layers</a></td>
	<td align="center" colspan="2"><a href="./docs/PERFORMANCE_SNAPSHOT.md">Performance/Resource Utilization</a></td>
</tr>
  <tr></tr>
<tr>
	<td align="center" colspan="4"><a href="./docs/PERFORMANCE_EVAL.md">Performance Eval</a></td>	
</tr>
<tr></tr>
    <tr></tr>
  <tr><th colspan="6"></th></tr>

  <tr></tr>
  <tr>
     <th rowspan="7" width="17%">Design and Development</th>
   </tr>

<tr>
	<td  align="center"><a href="./docs/API.md">API Reference</a></td>
	<td  align="center"><a href="./docs/QUANTIZATION.md">Quantization User Guide for CHaiDNN</a></td>
	<td  align="center"><a href="./docs/MODELZOO.md">Model Zoo</a></td>
	<td  align="center"><a href="./docs/RUN_NEW_NETWORK.md">Running Inference on new Network</a></td>
</tr>
  <tr></tr>
<tr>
	<td  align="center"><a href="./docs/BUILD_USING_SDX_GUI.md">Creating SDx GUI Project</a></td>
	<td  align="center"><a href="./docs/CONFIGURABLE_PARAMS.md">Configurable Parameters</a></td>
	<td  align="center"><a href="./docs/CUSTOM_PLATFORM_GEN.md">Custom Platform Generation</a></td>
	<td  align="center"><a href="./docs/SOFTWARE_LAYER_PLUGIN.md">Software Layer Plugin</a></td>
</tr>
  <tr></tr>
<tr>
	<td  align="center" colspan="2"><a href="https://www.xilinx.com/support/documentation/sw_manuals/xilinx2017_4/ug1027-sdsoc-user-guide.pdf">SDSoC Environment User Guide</a></td>	
	<td align="center" colspan="2"><a href="./docs/HW_SW_PARTITIONING.md">Hardware-Software Partitioning for Performance</a></td>

</tr>  
</table>

## Introduction

CHaiDNN is a Xilinx Deep Neural Network library for acceleration of deep neural networks on Xilinx UltraScale MPSoCs. It is designed for maximum compute efficiency at 6-bit integer data type. It also supports 8-bit integer data type.

The design goal of CHaiDNN is to achieve best accuracy with maximum performance. The inference on CHaiDNN works in fixed point domain for better performance. All the feature maps and trained parameters are converted from single precision to fixed point based on the precision parameters specified by the user. The precision parameters can vary a lot depending upon the network, datasets, or even across layers in the same network. Accuracy of a network depends on the precision parameters used to represent the feature maps and trained parameters. Well-crafted precision parameters are expected to give accuracy similar to accuracy obtained from a single precision model.
