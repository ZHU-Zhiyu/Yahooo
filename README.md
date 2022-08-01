Reviewer #mj7a
Thanks for your time and efforts in reviewing our submission, as well as the valuable comments. The authors appreciate your recognition of our work. In the following, we will address all of your concerns point-by-point in detail.

Q1. Differences from point cloud trackers.
**Response**: Although point cloud (PC) and event data indeed share some similarities, e.g. sparsity, the authors think that there is a considerable gap between the event-trackers and PC-trackers:
From the perspective of data characteristics: the major difference of the scale, between PC(~1K points per sample) and event data (>10K points per sample), makes it nearly impossible to directly adopt PC-trackers for event-based tracking. Meanwhile, the PCs describe the “shell” of objects in 3d coordinate space, whose interiors are common to be empty and usually not critical. However, event data measures the intensity variations in 3d spatio-temporal space, which are equally important. Thus, widely adopted distance-farthest point sampling (FPS), e.g., point-net ++ , does not make much sense for event data. Consequently, designing a new sampling method for event data is essential. 
The authors agree that any sparse point-sets could be processed as 3d point clouds. However, there are fundamental differences between event data and point clouds. Point clouds describe the surface of 3d entities with geometric information. However, the even data measures the pixel intensity variations in 3d spatio-temporal space. Thus, .
物理意义不一样

On the basis of model designing: as the reviewer mentioned, authors have aggregated the inherent semantic and motion information for object tracking, which is fundamentally different from SOTA PC-trackers. Meanwhile, the proposed method takes advantage of an image backbone to extract semantic-rich embeddings. Furthermore, the aforementioned sampling method is also very different from current PC-trackers. 

More importantly, we also conducted experiments to compare the proposed method with SOTA PC-trackers on the FE108 dataset. Specifically, we trained the PC-trackers with the same experiment settings as ours, and we also tried our best to tune their parameters to achieve optimal performance. As shown in the following table, it can be seen that the proposed method exceeds the PC-trackers [1, 2] to a large extent, i.e., 7.4% and 15.5% improvements in terms of the success rate and precision respectively, which demonstrates the demand of designing event-specialized tracking algorithms. We will add some discussions about the relation between event- and PC-trackers in the camera-ready version.
|  	 		 | RSR | OP_{0.50}| OP_{0.75}| RPR
|----	 		 | ----	| -----          |-----       	  |-----
|P2B CVPR’20 [1]	 |36.1	| 35.4 	       | 4.7        	  |40.5
|PTTR CVPR’22 [2]	 |47.5	| 49.6          | 11.5      	  |70.2
|Ours     		 |54.9   | 65.8          | 21.4         |85.9

.

Q2. Ablative study towards semantic-aware target likelihood . 
**Response**:  In the ablative study (Table 3 of the manuscript), we alway reserve the semantic-aware target likelihood module under different cases because we think it  is a basic and necessary module in the framework to achieve object tracking. Following your suggestion, we also conducted such an ablation study on the FE108 dataset, i.e., removing this module with all the other modules unchanged.   As listed in the following table, it can be seen that the tracking performance decreases significantly.

|            				  | RSR | OP_{0.50}| OP_{0.75}| RPR
|----	 				  | ----	| -----          |-----       	  |-----
|W/O semantic-aware target likelihood|45.2   | 43.5	       | 10.2         | 65.3
|Full model                 			 |54.9   | 65.8          | 21.4         | 85.9


Q3.Comparison with E-PrDiMP. 
**Response**: Note that E-PrDiMP refers to the modified PrDiMP (an RGB-based tracker) to adapt to event data in the form of the frame representation. In the HDR scenario of the FE108 dataset, the objects have relatively slow movement, making that the edges of the objects in the event-frame are relatively clear. Thus, the event frame-based method E-PrDiMP may grasp the semantic meaning of objects better, leading to its higher tracking performance. Meanwhile, the higher tracking performance of  E-PrDiMP than PrDiMP in the LL and HDR scenarios also supports our statement that “event-based generally surpass RGB-based methods under LL and HDR scenarios”. Such an advantage is credited to the high dynamic range characteristic of the event camera, as mentioned in Lines 30 – 32 of our manuscript, i.e., it could capture pixel intensity variation under very poor light conditions, while traditional RGB cameras may even have no response under these conditions. We will add the corresponding discussion in the camera-ready version.

[1] Qi H., Feng C., Cao Z., et al. P2b: Point-to-box network for 3d object tracking in point clouds, in Proc. IEEE/CVF CVPR, 2020, pp. 6329-6338.
[2] Zhou C., Luo Z., Luo Y., et al. PTTR: Relational 3D Point Cloud Object Tracking with Transformer, in Proc.  IEEE/CVF CVPR, 2022, pp. 8531-8540.

Reviewer #bkyP
Thanks for your time and efforts in reviewing our submission, as well as the valuable comments. The authors appreciate your recognition of our work. In the following, we will address all of your concerns point-by-point in detail.

Q1. Design for high temporal resolution & sparsity. 
**Response**: The traditional event frame-based trackers accumulate events into event-frames, inevitably compromising the temporal resolution, while the proposed method directly consumes raw event data (or event clouds), which can naturally preserve original high temporal resolution. Moreover, the sparse and asynchronous sensing manner (i.e., sparsity) results in that the underlying structure of event clouds is irregular (Lines 134 – 135 of the manuscript).  To deal with such an irregular characteristic, we propose to adopt the graph neural network (GNN) to embed the spatio-temporal information. Compared with the event frame-based method, which convolves on both triggered and inactivated pixels, our GNN-based method directly processes the sparse event points, which is more plausible for event data processing Besides, to handle the huge data samples caused by the raw event data efficiently, we propose a simple yet effective downsampling strategy.  Finally, the subsequent modules, including target likelihood prediction via semantic-driven siamese-matching and motion trajectory back-tracking, object proposal, as well as the training strategies, are all specially designed to adapt to event cloud data. Note that in Table 3 of the manuscript, we experimentally validated the effectiveness of each module.

Q2. Effectiveness of using raw event data. 
**Response**: Note that all modules and strategies of our framework are carefully designed for directly processing raw event data, e.g., the key-event sampling algorithm, key-event embedding graph neural network, key-event back-tracing, etc. Thus, the proposed method cannot be fed with other event representations. Thus, the superior performance of the proposed method, compared with SOTA methods, can validate the effectiveness of directly using raw event data. The authors also refer the reviewer to the 1), 4), and 6) in Table. 3 that the specially designed modules make a really great improvement for the proposed framework.

Q3. Baseline models.
**Response**: To the best of our knowledge, currently, there is a very limited number of works focusing on learning-based event tracking when submitting our manuscript, FENet [1] compared in Table 3 of the manuscript is the only officially published end-to-end learning-based event tracking work, whose source code is also publicly available. 

- We did not compare with model (or non-learning) -based methods because most of them  did not release their source codes. Besides, for model-based methods, hyperparameters have to be tuned for each specific sequence, making it hard to achieve desired performance on the large-scale dataset. . 

- RGB data-based tracking has been studied for many years with great efforts devoted, and many mature techniques have been proposed, producing impressive performance. Thus, we believe it is convincing to construct event tracking baselines by modifying the strong RGB-trackers to adapt event data . Note that such a manner was also adopted in Ref. [1]. As shown in Table 3 of the manuscript, the modified event-trackers from RGB-trackers, i.e., E-PrDiMP and E-TransT, which consume event-frames,  achieve reasonable performance.  

- Finally, we also want to notice that the leftmost four methods in Table 3, i.e., ATOM, DiMP, SiamFC++, and PrDiMP, take the corresponding RGB data as input, which are provided to illustrate the differences between RGB data-based tracking and event data-based tracking 

Q4. Key-event back-tracing models. 
**Response**: Sorry for the confusion caused. The key-event back-tracing module is designed to back-trace each key-event to the place, where it should be, in the 2-D initial x-y plane whose time is the initial time of the corresponding event cloud sample. Thus, we could compare back-traced key-events with the previous bounding box, as those key-events have been coordinated to be with the same timestamp with the bounding-box. Meanwhile, the time differences could be easily calculated, i.e., the current key-event time-stamp minus the initial time of this event cloud sample.

[1] Zhang J, Yang X, Fu Y, et al. Object tracking by jointly exploiting frame and event domain, in Proc. IEEE/CVF  ICCV, 2021, pp. 13043-13052.

Reviewer #Vqd3
Thanks for your time and efforts in reviewing our submission, as well as the valuable comments. The authors appreciate your recognition of our work. In the following, we will address all of your concerns point-by-point in detail.

Q1. Key-event sampling algorithm.
**Response**: The spatially closest events are always chosen as the key-event. Note that,XXXXXX For the special case that two events are within the same distance, the framework would randomly choose one as the key-event. Meanwhile, the ablation studies, i.e, 4) and 6) of Table 3 of the manuscript, also validate the effectiveness of the designed sampling algorithm. For the potential temporal information compression, it may occur in the downsampling procedure. However, it’s not so serious for the proposed framework. We further investigate such a compression via counting the total number of points perceived by the GNN, including the key-events and the utilized neighbor events.  with the total number of points in event cloud samples in each time interval.

The table statistically shows that the sampling algorithm preserves most temporal information, which contains more than 87% points in each time interval. Thus, although there are indeed xxXXx the authors believe that the intrinsic high-temporal resolution has not been heavily compressed.

**Response**: As stated in line 160 of the paper, we choose the spatially closest events as the key event, and the empty grid is the same strategy. For the special case that two events are within the same distance, the framework would randomly choose one as the key-event. Note that we use the grid to sample key points and then rearrange the spatio-temporal embedding of these key points to form a 2D matrix for the subsequent transformer. Thus, our method can also effectively solve the particular case of multiple sampling at the same event. For the potential temporal information compression, it may occur in the downsampling procedure. However, it’s not so serious for the proposed framework because the GNN-based spatio-temporal embedding utilizes the adjacent information around the key events. We further investigate such a compression via counting the ratio of perceived events (including the key events and the utilized neighbor events) by the GNN in each time interval. The following table shows that our method can perceive more than 87% of events in each time interval, preserving most temporal information. Moreover, the ablation studies, i.e., 4) and 6) of Table 3 of the manuscript, also validate the effectiveness of the designed sampling algorithm.  
|Normalize time range    | [0,0.1) | [0.1,0.2)| [0.2,0.3)| [0.3,0.4)| [0.4,0.5)| [0.5,0.6)| [0.6,0.7)| [0.7,0.8)| [0.8,0.9)| [0.9,1.0]
|  ----	 	        |-----     |-----        |-----         |-----        |-----        |-----         |-----        |-----        |-----        |-----     
| #Events in event clouds	           |100301 |102215  |101460  |98821    |99518    |101035|100909	|100071 | 97715   | 97955
|  #Events been perceived by the GNN  |88176  |92543  |93668  |92450    |93803	|95285	|94575   | 92803  | 89008     | 86761
| Ratio of perceived events	            | 87.91  | 90.54  |92.32   |93.55	|92.26	|94.31	|93.72   |92.74     |91.09       |88.57
Q2. Technical details.
**Response**: Sorry for the confusion caused. As claimed in the manuscript, we make the first attempt to construct a new end-to-end learning-based paradigm that directly consumes event clouds. Accordingly, we build the whole framework from scratch, rather than on the basis of an existing pipeline. Thus, there are too many technical details different from previous works. Due to space limitations, we mainly put the motivation, the function, and the brief implementations of each module in the manuscript so that the proposed paradigm can be grasped from a more global perspective. Regarding the detailed implementations, we have to move them to the supplementary material. Besides, to help the reviewers understand the method better, we also submitted the source code. Finally, according to the NeuIPS policy, i.e., “ If your submission is accepted, you will be allowed an additional content page for the camera-ready version,” we will add more details in the camera-ready version to make the paper better understood. 

Q3. Simulation method.
**Response**: There indeed exists a gap between the simulated and real events. Thus, during the Evt-LaSOT comparison experiment, we only want to demonstrate a “relative superiority” of the proposed method compared with the other event-based methods for the tracking on **non-rigid** objects. Because, all the event-based methods have to tolerate such simulation distortions. Meanwhile, such a simulation setting has been widely adopted in the field of event processing.

Q4. Different event representations.
**Response**: Note that our method is designed for directly processing raw event data (or event clouds), and it cannot consume event data with other representations via post-processing. Besides, Ref. [1] has  experimentally validated that the event-frame-based representation produces better performance than Time Surfaces, Event count, Time-Surface with Linear Time Decay Frames . (we refer the reviewer to Table 4 J, K, L, M,and N  of [1] for more details.) 

Q5.Supervision of flow estimation module.
**Response**: Sorry for the confusion caused. We refer the reviewer to Sec. 3 of Supplementary Material for the details of the regularization term $L_{CD}$ involved in Eq. (5) of the manuscript, which is used for supervising the flow estimation module. The event cloud within a very tiny time interval at the beginning of each event cloud sample is utilized as the potential locations of the back-traced key-events. We use Chamfer Distance to measure the discrepancy between two point sets. Moreover, considering that  some key-events may be missing, or some noise events may be sampled in such a potential target set, which may disturb the learning process, we also truncate the  accidental errors using a threshold.

Q6.Structure of Fig. 3.
**Response**: Sorry for the confusion caused. The four subfigures (a), (b), (c), and (d) indicate the tracking results of different methods on four event cloud samples from different sequences. Besides, we also agree with the reviewer that there are indeed some noises (or errors) in the predicted motion vectors. However, the majority of the motion vectors are consistent with the movement direction of the target object (see the video demo contained in Supplementary Material). Besides, it is worth noting that most of the noisy motion vectors correspond to the noisy events. As the ground-truth motion vectors are unavailable  for this dataset, we cannot evaluate the prediction quality quantitatively.  We also want to notice that the predicted motion vectors are by-product, and although our method does not predict completely accurate motion vectors, such a motion flow estimation module indeed makes a contribution, i.e., boosting the  (see the ablation studies in 3) and 6) of Table 3 of the manuscript). Last but not least, Fig. 3 is mainly used to demonstrate that the motion flow estimation module indeed works as what we claim, i.e., predicting motion vectors.
Q7.  Tracking speed.
**Response**: As mentioned in the **Response to Q3**, we only want to demonstrate a “relative superiority” of the proposed method compared with the other event-based methods for the tracking on non-rigid objects. We also refer the reviewer to the lines 277-280 of the manuscript for the reason of only comparing with event-based methods. In the Evt-LaSOT experiment, all the event data is synthesized from the RGB-data, which do not have high temporal resolution and HDR characteristics. Meanwhile, the optical flow of the simulator may not correctly reflect the real object movement. Thus, the authors think that it’s not fair to compare event-trackers with RGB-trackers. Furthermore, the SOTA RGB-trackers utilize more large-scale training datasets, e.g., TrackingNet, LaSOT, COCO, and GOT-10K, which is also unfair for event-based methods.

Q8. FENet performance.
**Response**: Note that FENet is a multi-modality method, which consumes both RGB and event data to achieve tracking. As mentioned in **Line 249** of our manuscript, for a fair comparison, we compared the proposed method only with the event branch of FENet (its performance is listed in Table 4 B of [1]). Besides, we also want to note that we retrained the event-branch of FENet and obtained a higher RSR (52.3) than that reported in the original paper (i.e., 52.0). Thus, we confirm the results of FENet are correct.

[1] Zhang J., Yang X., Fu Y., et al. Object tracking by jointly exploiting frame and event domain, in Proc. IEEE/CVF ICCV, 2021, pp. 13043-13052.

Reviewer #kchd
Thanks for your time and efforts in reviewing our submission, as well as the valuable comments. The authors appreciate your recognition of our work. In the following, we will address all of your concerns point-by-point in detail.

Q1. Meaning of each subfigure in Fig. 3.
**Response**: Sorry for the confusion caused. The upper row compares the tracking accuracy of different methods on four different event cloud samples. Meanwhile, the bottom row denotes a local zoom-in view of the dashed box in corresponding sub-figures of the upper row. The green arrows start from the spatial locations of key-events and point to the direction of motion flow. We also clarify the thickness difference of those arrows does not mean anything and is only caused by the different ratios of zoom-in local-views. We will update this figure in the camera-ready version. 

Q2. Sequence length.
**Response**: In the following table, we list the details of the test dataset.  We also categorize  the performance of our method based on the sequence length in the following tables. Besides, the results of the second-best method Trans-T are also provided for comparisons.

| 	      | Min length      | Max length| Avg length
|----	      | ----	       | -----            |-----
|FE108 	      | 641	       | 2400	   | 1864
|Evt-LaSOT| 1260	       | 4499	   | 2372

|FE108 		|Method| RSR| OP_{0.50}| OP_{0.75}| RPR
|----	      	| ----	 |-----   |-----            |-----            |-----
|Seqs. #Frames < 1000|Trans-T|70.4  |91.7        | 40.0        | 100.0 
|Seqs. #Frames < 1000|Ours     |70.5  | 93.1	     | 39.8         | 100.0
|Seqs. #Frames > 2000|Trans-T |49.4 |55.9         |21.2	| 78.3
|Seqs. #Frames > 2000|Ours     |54.2  | 63.9	     | 21.4         | 85.4
|All Seqs.          |Trans-T |52.4  | 62.2    | 21.0         | 87.0  
|All Seqs.          |Ours     |54.9  | 65.8     | 21.4         | 85.9

|Evt-LaSOT	|Method| RSR | OP_{0.075} | RPR
|----	      	| ----	 |-----    |-----               |-----     
|Seqs. #Frames < 1500|Trans-T |41.0   | 27.2    | 52.2
|Seqs. #Frames < 1500|Ours      |39.4   | 23.3    | 47.9
|Seqs. #Frames > 3000|Trans-T |27.2   |  11.0    | 18.1
|Seqs. #Frames > 3000|Ours 	 |31.0   | 21.5     | 34.1
|All Seqs.                    |Trans-T |30.3  |  17.9    | 30.1
|All Seqs.                    |Ours	 |32.3   | 22.1     | 35.9

From the above tables, it can be seen that our method generally performs better on relatively short sequences than on relatively long sequences, which is also a common phenomenon for the object tracking task. On the relatively long sequences, our method consistently outperforms Trans-T to a large extent, demonstrating the advantage of our method.  Besides, in Lines 312 – 313 of the manuscript, we indeed discussed that more efforts and studies should be devoted to improving the performance on extremely long sequences, which is more challenging.

Q3. Evt-LaSOT construction.
**Response**: We randomly selected 8 sequences from the original LaSOT dataset, i.e., bird-15, bird-3, crab-6, crab-18, cat-20, cat-3, crocodile-3, bear-4. The statistics of these sequences are provided in the above table (**Response to Q2**). In the camera-ready version, we will clarify this issue.

Q4. Algorithm efficiency.
**Response**: We confirm that the high efficiency of our method is mainly credited to that it consumes raw event data without pre-processing. Specifically, we utilized the officially released code by FE108 for pre-processing, i.e., converting raw event data into event frames. The average time of pre-processing one event-frame is about 33.8 ms. However, for our method, such a pre-processing step is not required, and its total running time is about 39.7 ms, which validates the efficiency of our method. 

Q5.  Meaning of each sub-screen in supplementary video.
**Response**: For the meanings of each sub-screen, we have denoted it in the first 2 seconds of the video: the main screen shows the tracking comparison; the other three sub-screens in the right-side show, from top to the bottom, motion-aware target likelihood, semantic-driven target likelihood, and motion flow, respectively.

Q6. Limitations:  security and robustness in industrial applications.
**Response**: Comparisons with RGB-based methods: TtThe authors agree that RGB data-based tracking indeed is an important part for industrial applications. However, event data-based methods have shown its potential under the HDR and low-light scenarios, which may further improve the whole system, together with the RGB data-based tracking. Here we  do not mean that event-based sensing will replace the RGB-based sensing. The authors think that current event-based techniques are in the developing phase, which is common to a new technique. Note that an Autopilot Tesla Model 3 has killed a motorcyclist on southbound I-15 at 1 a.m. Jul/24/22’, which may be caused by the poor lighting conditions in the early morning. 
