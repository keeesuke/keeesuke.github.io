
## Projects

### Risk-Aware Autonomous Driving with Pedestrian Intent Prediction

##### [Full Project Description](/assets/img/UIUC/principal_autonomy/Autonomous_GEM_Project.pdf)

ROS 2–based autonomous driving stack deployed on the UIUC GEM e4 electric vehicle, designed to improve pedestrian safety by adapting vehicle behavior using time-to-collision (TTC) risk estimation rather than distance-only braking. The system integrates LiDAR- and RGB-D–based pedestrian perception, weighted sensor fusion with approximate time synchronization, pedestrian motion prediction, and a high-level decision state machine that gates safety-aware control, enabling earlier and more robust speed adaptation under uncertainty and intermittent detections.

<img src="/assets/img/UIUC/principal_autonomy/GEMe4.gif" width="680"/>

<hr>

### Open-WSemantic Zero-Shot 6D Pose Estimation Using SAM3 and FoundationPose

##### [Full Project Description](/assets/img/UIUC/computer_vision/6D_Pose_Estimation_Project.pdf)

This project presents an open-vocabulary, zero-shot 6D object pose estimation framework for robotic manipulation that enables language-guided target selection and dynamic switching in unstructured environments. Built on FoundationPose (CVPR 2024), the pipeline integrates SAM-3 for precise segmentation and a lightweight vision-language model to support text-driven object specification, while removing reliance on pre-registered CAD models through on-the-fly geometric proxy generation, enabling robust 6D pose tracking even under significant occlusion.

<img src="/assets/img/UIUC/computer_vision/6D_pose_estimation.gif" width="450"/>
<p style="font-size:10px"><br>
reference: nvlabs.github.io/FoundationPose</p>

<hr>

### Box Consolidation with Genetic Algorithm
##### [Full Project Description](/projects_at_amazon/box_consolidation/box_consolidation)

Box Consolidation Project aimed at reducing unnecessary box types used in Amazon Fulfillment Centers (FC) to improve shipment efficiency. There were 400 box types managed across FC network, with many rarely used boxes and redundant similar sizes complicating inventory and data management. The Box Consolidation will align box suites by FC type, consolidate boxes, halt less-used box production, and optimize air ratio; it is initially being implemented across all FCs in Japan where it is expected to reduce cube per shipment by 3.2% and lead to $770,000 in annual savings. I employed a Genetic Algorithm to swiftly calculate optimal box lineups specialized for each FC type out of the vast number of possible box combinations.

<img src="/assets/img/box_consolidation/box_consolidation.png" width="600"/>

<hr>

### SIOC ML Prioritized Inventory Expansion Program
##### [Full Project Description](/projects_at_amazon/SIOC_ML/sioc-text-ml-project)

Ship in Own Container (SIOC) is a shipping type that reduces costs by shipping items without Amazon packaging. To increase SIOC shipments, our team provides a prioritized list of products to inspect if they meet SIOC standards. I developed a machine learning model to predict SIOC likelihood and prioritize inspections to maximize future SIOC shipments. The model was trained on historical data, using 587k data points and techniques like Word2Vec and Topic Modeling. The SIOC inventory expansion program, enabled by the ML prioritization model, is estimated to drive over 500M JPY in cost savings anually.

![SIOC image](/assets/img/SIOC_ML/SIOC.png)
<hr>

### Box Dimensional Size Optimization with Simulated Annealing
##### [Full Project Description](/projects_at_amazon/box_opt_VDB/box_opt_VDB)

One of our team goals is to contribute to sustainability by reducing CO2 emissions and plastic usage through packaging optimization. Variable Depth Box (VDB) is a packaging solution designed to adjust its height based on the size of the product, reducing air space and minimizing packaging material waste. I developed a tool to optimize the three-dimensions of VDB sizes with Simulated Annealing, one of optimization algorithm, and demonstrated a 2.16% improvement in the air ratio, based on the annual shipment record. While the simulated annealing proved effective in finding optimal box size lineup, there are some rooms for further algorithm refinement such as slow convergence and sensitivity to initial parameters.

<img src="/assets/img/box_opt_VDB/opt_curve.png" width="500"/>
<hr>

### Monitoring System Deployent
##### [Full Project Description](/projects_at_amazon/monitoring_system/monitoring_system)

Seiren is an in-house visualization system by Amazon Japan for monitoring Fulfillment Center (FC) equipment. It provides real-time and historical data on operational status, productivity, and errors, extracted from PLCs controlling warehouse conveyors. The system minimizes delays in identifying issues, preventing operational process halts and delivery risks by alerting on-site associates to abnormalities. I, as a program manager, led the Seiren project end-to-end from system design to deployment and subsequent maintenance, and successfully deployed the system across 6 new FCs over the course of two years.

<img src="/assets/img/monitoring_system/UI_sample.png" width="500"/>

<hr>

### Similar Product Recommendation Tool
##### [Full Project Description](/projects_at_amazon/Word_Similarity_Tool/word_similarity_tool)

The Similar Product Recommendation Tool is designed to streamline the identification of incorrectly annotated products (e.g., items marked as liquid/fragile but not meeting such criteria). It leverages TF-IDF and Cosine Similarity to accurately match input keywords with product titles or descriptions, reducing manual inspection efforts. The tool processes keyword searches via an AWS-backed architecture, providing results through a Python-based Flask server. I designed and developed the User Interface and the backend system architecture for our team to utilize for their internal use case. Potential broader applications include identifying SIOC-eligible products to cut packaging costs and mitigating risks for damaged items by analyzing similarities.

<img src="/assets/img/Word_Similarity_Tool/System_architecture.png" width="600"/>

<br>

## Education

### University of Illinois, Urbana-Champaign 
<p style="text-align:center">
    Master of Engineering in Autonomy & Robotics <br>
    August 2025 ‒ (Exp.) Dec 2026
</p>

- Professionally oriented program focusing on software systems, electronics, machine learning, dynamics, control, and sensor integration for autonomous systems.
- <b>Relative courses</b>: Principles of Safe Autonomy, Computer Vision, Mobile Robotics, Intro to Robotics

<p align="center">
  <img src="/assets/img/UIUC/safe_autonomy.png" width="140" style="background-color:white; padding:3px;"/>
  <img src="/assets/img/UIUC/safe_autonomy2.gif" width="140" style="background-color:white; padding:3px;"/>
  <img src="/assets/img/UIUC/ur3.png" width="160" style="background-color:white; padding:3px;"/>
  <img src="/assets/img/UIUC/cv_inlier.png" width="180" style="background-color:white; padding:3px;"/>
</p>

<hr>

### Yokohama National University
<p style="text-align:center">
    B.E., Material Science and Mechanical Engineering <br>
    Apr 2014 ‒ Mar 2019
</p>

- A program focusing on structure and properties of materials
- Undergraduate research: Analyzed contributors to the stability of retained austenite in low alloy TRIP (transformation-induced plasticity) steel using machine learning
- <b>Relative courses</b>: Mechanics of Materials, Crystallographic Strength, Metallography

#### Publications
- Norimitsu Koga, Takayuki Yamashita, Keisuke Ogawa, Osamu Umezawa: [Statistical Analysis of Influential Factors on the Stability of Retained Austenite in Low Alloy TRIP (transformation-induced plasticity) Steel](https://www.jstage.jst.go.jp/article/matertrans/63/5/63_MT-M2021239/_article/-char/en). Materials Transactions, 2022 Volume 63 Issue 5 Pages 693-702; DOI: 10.2320/matertrans.MT-M2021239


<hr>

### University of Edinburgh
<p style="text-align:center">
    Full Year Courses for Visiting Students, School of Informatics <br>
    Sep 2016 ‒ May 2017
</p>

- A year-long study abroad program, focused on the fundamentals of data science.
- <b>Relative courses</b>: Computation and Logic, Functional Programming, Data and Analysis

<br>

## Work Experience

### Preferred Robotics
<p style="text-align:center">
    <b>Robotics Software Engineer @ software team</b> <br>
    May 2025 - Aug 2025
</p>
Worked on the development of an in-house mobile inspection robot operating along railway environments, with a focus on perception-driven autonomy. Led the development of 3D environment modeling using Gaussian Splatting and implemented real-time camera control pipelines in C++. In parallel, designed collision prediction logic for dynamic obstacles to improve autonomous navigation and path planning in safety-critical infrastructure settings.

<hr>

### Amazon Japan

<p style="text-align:center">
    <b>Sr.Engineer @ Customer Packaging Experience team</b> <br>
    Nov 2019 - Feb 2021, Dec 2023 - Nov 2024
</p>
Worked on large-scale warehouse automation and logistics optimization across robotics, machine learning, and control systems. Contributed to robotic arm–based packing automation using deep learning–based semantic segmentation, as well as multimodal ML systems combining text, image, and physical attributes to identify shipping-ready products at scale. Led optimization initiatives including box consolidation via genetic algorithms and variable-depth box sizing using simulated annealing, delivering measurable cost and sustainability improvements.

<p style="text-align:center">
    <b>System Engineer @ Control Engineering team</b> <br>
    Apr 2019 - Oct 2019, Mar 2021 - Nov 2023
</p>
Designed and deployed warehouse monitoring and operational tools integrating PLC data, real-time dashboards, and cloud-based infrastructure to support nationwide fulfillment operations.

<hr>

#### Datumix Corp.

<p style="text-align:center">
    <b>Full time internship @ Tokyo Japan/California U.S.</b> <br>
    Sep 2018 - Mar 2019 <br>
</p>
Simulated the unmanned Multi-Shuttle System for warehouse loading and enhanced the efficiency of shipping order for multi-order items by applying reinforcement learning.

<br>

## Hobbies
- Drum, Football, Work-out, Swimming, Exploring Abroad

Me playing the drum in the studio! <br>
<img src="/assets/img/personal/drum.PNG" width="400"/>


<hr>
<p style="font-size:11px">*Credential*<br>
In compliance with company policies and the Code of Conduct, no confidential information is included, nor are any images of actual products or source code developed during these projects presented. This document has been created independently by Keisuke Ogawa and is not affiliated with or owned by Amazon Japan.</p>