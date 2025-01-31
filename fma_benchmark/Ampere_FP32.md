# Background
In the Ampere Architecture （as released https://images.nvidia.com/aem-dam/en-zz/Solutions/geforce/ampere/pdf/NVIDIA-ampere-GA102-GPU-Architecture-Whitepaper-V1.pdf), an extra FP32 pipeline is added and Nvidia is claiming it has 2X-FP32 processing as below.  
![image](https://user-images.githubusercontent.com/2059536/154828748-ba458ecc-d491-4217-8cce-a9f935a236be.png)

It is interesting to see how the extra FP32 pipeline is used along the normal FP32 pipeline. There are 2 choices from the architecture point of view.

- Use the extra FP32 pipeline as the normal pipeline whereby the wider instances can be issued to this pipeline.Effectively double the warp width (which should be from 16 to 32 in this context).  This will lead to the highest utilization of the pipeline but with a higher hardware bill cost. 

- Use the extra FP32 pipeline as the co-issue pipeline whereby you can only use it when there are more than 2 "streams" which are independent of each other. This architecure demands relatively simple hardware design but not an "efficient use" of the hardware resource actually. But presumably in the modern gaming workload there might be a few such opportunisitical windows for this to be used.
 
# Configuration
We use the following hardware to dissect the truth of the Ampere architecture. 


3060Ti(GA104) | A2000（GA106)
--- | ---
![image](https://user-images.githubusercontent.com/2059536/154828954-8784cbf6-0810-4492-a02f-6890b5c5309c.png)  | ![image](https://user-images.githubusercontent.com/2059536/154829274-2f71fb10-3769-4f98-adc7-df5ff563d581.png)


# Results
We based our work on uVkCompute benchmark. We modifed the shader code to have the independent stream to test whether the extra pipeline can only be co-issued or a more of a native choice. There are 2 types of workload being crafted: 

- Type 0:there is only one stream of FMA workload dispatching to the shader
- Type 1:there are two streams of FMA which is dependent of each other 


FMA Dispatch( Type 0) | FMA Dispatch ( Type 1） 
|---|---|
|![image](https://user-images.githubusercontent.com/2059536/154829563-6309d675-d190-4eec-8deb-d729c85bbc92.png) | ![image](https://user-images.githubusercontent.com/2059536/154829570-d76f1222-a8b1-4665-82cc-691dcc2c5211.png) |





| <b>FMA Type0 workload Dispatch </b>|
|---|
|![image](https://user-images.githubusercontent.com/2059536/154829647-5b3bc8c3-a6d9-4e02-a153-2feb179b4b33.png ) |

| <b>FMA Type1 Workload Dispatch </b>|
|---|
|![image](https://user-images.githubusercontent.com/2059536/154830352-8f9e8d4b-334a-4b10-97ca-aa622117eff3.png)|


# Conclusion

It appears that only the <b> independent streams </b> can take advantage of the <b> extra FP32 pipeline </b> as shown in the following table. Therefore, it should have been done in the "coissue" way.  

|   | FP32/FP16|
|---|---|
|FMA Dispatch Type0 | 1X|
|FMA Dispatch Type1 | 2X|



