# OpenVINO-Samples
This is a list of samples to run on different hardware.<br>

CPU requires FP32 or int8 models.  All other hardware requires FP16 models, though GPU can run non-optimally with FP32 models in some cases.

To prepare for the exercise, we're going to convert an FP32 SqueezeNet 1.1 model (provided with OpenVINO) to an FP16 version.


<h3>Run a Sample Application</h3>

<ol>
	<li>Create a directory for the FP16 squeezenet model.
		<pre class="brush:bash; class-name:dark;">mkdir ~/squeezenet1.1_FP16</pre>
	</li>
	<li>Move into the new directory.
		<pre class="brush:bash; class-name:dark;">cd ~/squeezenet1.1_FP16</pre>
	</li>
	<li>Use the Model Optimizer to convert a model to FP16
		<pre>python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model ~/openvino_models/models/FP32/classification/squeezenet/1.1/caffe/squeezenet1.1.caffemodel --data_type FP16 --output_dir .</pre>
	</li>
	<li> Copy the labels file to the directory with the new FP16 SqueezeNet model.
		<pre>cp ~/openvino_models/ir/FP32/classification/squeezenet/1.1/caffe/squeezenet1.1.labels .</pre>
	</li>
	<li> Copy the car image to the samples directory for ease of use.
		<pre>sudo cp /opt/intel/openvino/deployment_tools/demo/car.png  ~/inference_engine_samples/intel64/Release</pre>
	</li>
	<li>Go to the samples directory:
		<pre class="brush:bash; class-name:dark;">cd ~/inference_engine_samples/intel64/Release</pre>
	</li>
	<li>Use the Inference Engine to run a sample application on the CPU or iGPU:
	<ul>
	<li> To run the sample application on the CPU:
		<pre class="brush:bash; class-name:dark;">./classification_sample -i car.png -m ~/openvino_models/ir/FP32/classification/squeezenet/1.1/caffe/squeezenet1.1.xml</pre>
	</li>
	<li>To run the sample application on the iGPU:
		<pre class="brush:bash; class-name:dark;">./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d GPU</pre>
	</li>
	</ul>

<li>To run the inference using both your FPGA and CPU, add the <code>-d</code> option and <code>HETERO:</code> to your target:
		<pre class="brush:bash; class-name:dark;">./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d HETERO:FPGA,CPU</pre>
		</li>

<li>To run the sample application on your target accelerator:
<br>
<br>		
<ul>
	<li> To run the sample application on the Intel® Arria® 10 GX FPGA Development Kit:
		<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk_2018.5.445/bitstreams/a10_devkit_bitstreams/5-0_A10DK_FP11_SqueezeNet.aocx</pre>
	</li>
	<li>To run the sample application on the Intel® Programmable Acceleration Card (PAC) with Intel® Arria® 10 GX FPGA:
		<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk_2018.5.445/bitstreams/a10_dcp_bitstreams/5-0_RC_FP11_SqueezeNet.aocx</pre>
	</li>
	<li>To run the sample application on the Intel® Vision Accelerator Design with an Intel® Arria 10 FPGA (Mustang-F100-A10):
		<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk_2018.5.445/bitstreams/a10_vision_design_bitstreams/5-0_PL1_FP11_SqueezeNet.aocx</pre>
	</li>
	<li>To run the sample application on the Intel® Vision Accelerator Design with Intel® Movidius™ VPUs:
		<pre class="brush:bash; class-name:dark;">./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d HDDL</pre>
	</li>
	<li>To run the sample application on the Intel® Movidius™ Neural Compute Stick or Intel® Neural Compute Stick 2:
		<pre class="brush:bash; class-name:dark;">./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d MYRIAD</pre>
	</li>
	</ul>

</li>
<p class="note"><strong>NOTE</strong>: The CPU throughput is measured in Frames Per Second (FPS). This tells you how quickly the inference is done on the hardware.</p>
<p>The throughput on the accelerator may show a lower FPS due to the initialization time. To account for that, the next step increases the iterations to get a better sense of the speed the inference can run on the accelerator.</p>
</li>
	<li>Use <code>-ni</code> to increase the number of iterations. This option reduces the initialization impact:
		<pre class="brush:bash; class-name:dark;">./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d HETERO:FPGA,CPU -ni 100</pre>
	</li>

</ol>
