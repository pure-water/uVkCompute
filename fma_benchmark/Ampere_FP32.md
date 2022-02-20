In the Ampere Architecture, an extra FP32 pipeline is added and Nvidia is claiming it has 2X-FP32 processing as below. 
![image](https://user-images.githubusercontent.com/2059536/154828748-ba458ecc-d491-4217-8cce-a9f935a236be.png)

It is interesting to see how the extra FP32 pipeline is used along the normal FP32 pipeline. There are 2 choices from the architecture of view.
 - Use the extra FP32 pipeline as the co-issue pipeline whereby you can only use it when there is dependancy. This one is relatively simple hardware design but 
 - Use the extra FP32 pipeline as the normal pipeline whereby the wider instances can be issued to this pipeline. This one has the highest utilization of the pipeline but with a higher hardware bill cost. 


We use the following hardware to disinsect the truth of the Ampere architecture. 


3060Ti(GA104) | A2000（GA106)
--- | ---
![image](https://user-images.githubusercontent.com/2059536/154828954-8784cbf6-0810-4492-a02f-6890b5c5309c.png)  | ![image](https://user-images.githubusercontent.com/2059536/154829274-2f71fb10-3769-4f98-adc7-df5ff563d581.png)



We based our work on uVkCompute benchmark. We modifed the code to test whether the extra pipeline can only be co-issued or a more of a native choice. 

FMA Dispatch( Type 0) | FMA Dispatch ( Type 1） 
|---|---|
|![image](https://user-images.githubusercontent.com/2059536/154829563-6309d675-d190-4eec-8deb-d729c85bbc92.png) | ![image](https://user-images.githubusercontent.com/2059536/154829570-d76f1222-a8b1-4665-82cc-691dcc2c5211.png) |

BenchMark | Type 0 Flops   | Type 1 Flops
|---|---|---|
|

![image](https://user-images.githubusercontent.com/2059536/154830478-c9175460-bbee-4377-ac8d-5620e3eea226.png)
![image](https://user-images.githubusercontent.com/2059536/154830491-a2aa93d6-6860-4814-b303-36589043790b.png)
![image](https://user-images.githubusercontent.com/2059536/154830497-c98f660c-9656-4aa3-b13d-d6b7f07ebd11.png)
![image](https://user-images.githubusercontent.com/2059536/154830525-c562bc83-df81-4aa0-826b-b9f57d33ce2e.png)
![image](https://user-images.githubusercontent.com/2059536/154830615-339e3d92-d2fd-4562-9961-f704a8d304ca.png)


|![image](https://user-images.githubusercontent.com/2059536/154829647-5b3bc8c3-a6d9-4e02-a153-2feb179b4b33.png "FMA Type0 workload )  | ![image](https://user-images.githubusercontent.com/2059536/154830352-8f9e8d4b-334a-4b10-97ca-aa622117eff3.png)|




