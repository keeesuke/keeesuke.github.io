<!--
How to convert word to markdown .md file:
$ pandoc <word_file_name.docx> -t markdown-raw_html-native_divs-native_spans -o <markdown_file_name.md>

Or this way:
$ pandoc -s 入力.docx --wrap=none --extract-media=media -t gfm -o 出力.md

-->

# Data Scientist
<div style="border: 2px solid #4CAF50; padding: 20px; border-radius: 10px;">

## Education	 			        		
- B.S., Material Science and Mechanical Engineering | The Yokohama National University (_Apr 2014 ‒ Mar 2019_)
- Exchange Program, School of Informatics | The University of Edinburgh in Scotland (_Sep 2016 ‒ May 2017_)

## Work Experience
**Sr.Engineer @ Amazon Japan, Customer Packaging Experience (_Nov 2019 - Feb 2021, Dec 2023 - Present_)**
- Led the development of Multi-Modal machine learning classification model using text and image data that identifies among over ten million products without the need of additional packaging materials, resulted in $9M of material and shipping cost savings.
- Employed genetic algorithms to compute 26 optimized box configurations from a shipping dataset of 500,000 items, contributing to a Cube Reduction equivalent to $700K by maximizing truck loading capacity.
- Installed cameras on warehouse conveyors to capture images of over 200,000 products, used image recognition to identify discrepancies in registered product sizes, and assigning smaller packages to save $500K annually in transportation costs.

**System Engineer @ Amazon Japan, Control Engineer (_Apr 2019 - Oct 2019, Mar 2021 - Nov 2023_)**
- Led the implementation of a monitoring system used by over 500+ users nationwide, overseeing the launch of 6 warehouses and managing all aspects from system design to development, testing, deployment, and maintenance.
- Developed a tool for warehouse operations using Python, empowering on-site team to monitor the remaining unpicked items for each truck shipment time, reduced $400K in unnecessary server expenses.
- Successfully migrated the MySQL community database used in the monitoring tool to Aurora MySQL RDS, implementing AWS auto-scaling functions which has resulted in a 20% reduction in CPU traffic.

**Full time internship @ Datumix Corp., Tokyo Japan/California U.S. (_Sep 2018 - Mar 2019_)**
- Simulated the unmanned Multi-Shuttle System for warehouse loading and enhanced the efficiency of shipping order for multi-order items by applying reinforcement learning.

## Projects
### Box Optimization with Genetic Algorithm to find Best Fitted Box Suite
[Full Project Description](/box-opt-project)

Box Optimization Project aimed at reducing and optimizing box types used in Amazon Fulfillment Centers (FC) to improve shipment efficiency. There were 400 box types managed across FC network, with many rarely used boxes and redundant similar sizes complicating inventory and data management. The Box Consolidation Approach will align box suites by FC type, consolidate boxes, halt less-used box production, and optimize air ratio; it is initially being implemented across all Sortable FCs where it is expected to reduce cube per shipment by 3.2% and lead to $770,000 in annual savings. The consolidation approach employs a genetic algorithm to swiftly determine optimal box lineups specialized for each FC type out of the vast number of possible box combinations.

![box_consolidation image](/assets/img/box_consolidation.png)

### SIOC ML Prioritized Inventory Expansion Program
[Full Project Description](/sioc-text-ml-project)

Ship in Own Container (SIOC) is a shipping type that reduces costs by shipping items without Amazon packaging. To increase SIOC shipments, our team provides a prioritized list of products to inspect if they meet SIOC standards. A machine learning model was developed to predict SIOC likelihood and prioritize inspections to maximize future SIOC shipments. The model was trained on historical data and improved over time, using 587k data points and techniques like Word2Vec and Topic Modeling. The SIOC inventory expansion program, enabled by the ML prioritization model, is estimated to drive over 500M JPY in cost savings anually.

![SIOC image](/assets/img/SIOC.png)


### Programming:
- Python, Shell script, C++, HTML, JavaScript

### Tools & Technologies:
- Machine Learning, Git, AWS (Sagemaker, RDS, EC2, S3), Hubble, ETL, Django, Flask, Docker, Linux, MySQL, NLP, MATLAB

### Languages:
- Japanese; native, English; business level (TOEFL 97/120, IELTS overall 7.5)

## Extracurricular Achievements:
- Programming Competition: Joined 6 competitions with a highest position of top 2.2% on [Signate](https://signate.jp/users/50394) / Attained a ranking A with a highest position of top 8% on Paiza / Solved 140+ problems on [Leetcode](https://leetcode.com/keyy1019/)
- Volunteer Work: Engaged in volunteer work for decontamination efforts in Fukushima following the tsunami of the Great East Japan Earthquake in 2011. Teaching school children in Ghana through Volunteer4Africa program on May 2017.

## Publications
- Norimitsu Koga, Takayuki Yamashita, Keisuke Ogawa, Osamu Umezawa: [Statistical Analysis of Influential Factors on the Stability of Retained Austenite in Low Alloy TRIP Steel](https://www.jstage.jst.go.jp/article/matertrans/63/5/63_MT-M2021239/_article/-char/en). 2022 Volume 63 Issue 5 Pages 693-702.

## Hobbies
Drum, Football, Swimming, Exploring Abroad

Me playing the drum in the studio!
![drum image](/assets/img/drum.PNG)


