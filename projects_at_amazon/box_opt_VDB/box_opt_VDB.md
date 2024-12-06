<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

## **VDB Optimization Project Background**

As part of Amazon's Climate Pledge initiative, our team is aiming to contribute to the reduction of CO2 emissions and plastic usage, as well as cost savings, by optimizing packaging size. To promote this initiative, our team has replaced a portion of the traditional cardboard box lineup with VDB (Variable Depth Box) to reduce the box volume upon delivery. VDB is a cardboard box that can adjust the height by folding, depending on the size of the packaged product. VDB can effectively create two different box sizes (as varying in height) with a single box, allowing for minimal air space within the box (as shown in figure 1).  
  
As of 2024, approximately 300,000 units of VDB are being shipped weekly across all Amazon Fulfillment Centers (FCs). However, the overall air rate at FCs has remained relatively unchanged at 48% since VDB's initial launch in early 2024. The primary issues identified are the lack of optimization in VDB heights and the limited girth range of the implemented VDB (where girth is defined as the sum of length, width, and height). Amazon's box types are categorized based on girth ranges: 60cm, 80cm, 100cm, 120cm, and above (e.g. if the girth falls between 60 and 80cm, that box is classified as the 80 girth range), and the girth range where VDB has been introduced as of today is only 60 to 80 girth.


**Figure 1. image of Variable Depth Box**

<img src="/assets/img/box_opt_VDB/VDB_image.png" width="237"/><img src="/assets/img/box_opt_VDB/VDB_image2.png" width="400"/>

The left image illustrates the box before and after folding to adjust its height, while the right image highlights the skip score lines at the folding points


Therefore, I developed a Japan original tool to optimize the size of VDB, including the folded height, to compare the air rate improvement against the existing VDB lineup. Additionally, I simulated the maximum opportunity of VDB when the target range is expanded from the current 60-80 girth to the VDB-capable 100 girth range. This will support the business team's decision-making on the number and sizes of the new VDBs to be introduced in 2025.

## **Result of Study**

Two patterns were examined regarding the VDB size optimization.  
**<u>Case 1:</u>**  
The 10 VDBs currently used in the 60-80 girth range have not been optimized for either the box size before folding or the height. I verified how much the air rate could be improved by replacing the existing VDBs with the optimized ones.  
**<u>Case 2:</u>**  
Instead of limiting the box to the current 60-80 girth range, I calculated the maximum opportunity when expanding the girth range. I verified how much the air rate could be improved by optimizing the extended 60-100 girth range.  
  
### **Assumptions**  
In Case 1, optimization was applied to 10 VDBs within the 60-80 girth range, while the remaining 16 boxes were left unchanged. In Case 2, the target girth range was expanded to 60-100, and optimization was conducted for 20 VDBs. A specific FC was selected for this analysis, ranking among the top three in VDB shipment volume across all JP FCs, making it a suitable candidate for optimization effort. In Case 2, the existing 100 girth range boxes were used as the baseline, where the height was simply halved, as the 100 girth VDB had not been introduced yet. The reason for using the halved height as the baseline is that it is the simplest manually determined height, and it has also been a reference for the VDB heights introduced so far. Optimization constraints shown below are determined by the factors such as the capacity of shelves at the packing stations, the physical limitations of folding the boxes, and the dimensions of boxes that can be manufactured by the box supplier.

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"></th>
<th style="text-align: center;"><strong>Case 1</strong></th>
<th style="text-align: center;"><strong>Case 2</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">Target shipment</td>
<td colspan="2" style="text-align: center;">~500,000 (random extraction from the past annual shipment of all FCs)</td>
</tr>
<tr>
<td style="text-align: center;">Min/Max box size</td>
<td colspan="2" style="text-align: center;">Aligned with an existing box’s min/max size</td>
</tr>
<tr>
<td style="text-align: center;">Number of total boxes in lineup</td>
<td colspan="2" style="text-align: center;">26</td>
</tr>
<tr>
<td style="text-align: center;">Number of VDB in lineup</td>
<td style="text-align: center;">10</td>
<td style="text-align: center;">20</td>
</tr>
<tr>
<td style="text-align: center;">Optimization target girth</td>
<td style="text-align: center;">60~80 girth</td>
<td style="text-align: center;">60~100 girth</td>
</tr>
<tr>
<td style="text-align: center;">Actual optimizing range</td>
<td style="text-align: center;">50~80 girth</td>
<td style="text-align: center;">50~100 girth</td>
</tr>
<tr>
<td style="text-align: center;">Optimization constraints in size</td>
<td colspan="2" style="text-align: center;"><ul>
<li><p>min length: 23.7cm</p></li>
<li><p>min width: 11.5cm</p></li>
<li><p>min height: 5cm (min folding height for VDB is 2cm)</p></li>
</ul></td>
</tr>
</tbody>
</table>

### **Results**  
For both cases, the optimization of 26 box lineups (including 10 VDBs) was based on annual shipment data, and the air rate before and after optimization was compared.

|        |                     | Air ratio  | Difference |
|:------:|:-------------------:|:----------:|:----------:|
| Case 1 | Existing Box Lineup |   40.08%   |    N/A     |
|        |  Optimized Lineup   | **38.65%** | **-1.43%** |
| Case 2 | Existing Box Lineup |   39.35%   |    N/A     |
|        |  Optimized Lineup   | **37.19%** | **-2.16%** |

The results showed an air rate improvement of -1.43% for Case 1 and -2.16% for Case 2. The larger air rate improvement in Case 2 is likely due to the fact that the number of boxes that could be optimized was doubled (from 10 to 20), providing more room for air rate optimization.

## **Approach and Methodology**

The traditional VDBs are manufactured for the 60-80 girth range, consisting of 10 VDBs, with the height folded to just half of the original height. However, this approach was not based on any calculations, thus not optimized. I identified the following three main challenges in creating the optimal VDB lineup:

<u>1. Choosing which boxes to replace:</u>

Instead of basing VDB sizes on existing box dimensions, creating entirely new three-dimensions will better fit the target shipments which has the same manufacturing effort and larger operational impact.

<u>2. Determining the ideal base box size before folding:</u>

We need to find the three-dimensional box size that minimizes the air rate, considering the number of boxes to replace and folding allowances.

<u>3. Deciding the ideal height for folding:</u>

After selecting the optimal base box size, we must determine the ideal folded height, while accounting for manufacturing constraints such as minimum and maximum folded heights.

I conducted the optimization by separating those three steps.

**Figure 2. Optimization process image separated by three steps**

<img src="/assets/img/box_opt_VDB/VDB_replacement_flowA.png" width="500"/>

<br>

### **Algorithm**

The problem of optimizing box sizes in logistics can generally be modeled as a "combinatorial optimization problem". Many combinatorial optimization problems are classified as NP-hard, meaning that finding an efficient solution can be extremely challenging as the number of possible combinations grows exponentially.  
  
Simulated Annealing is a metaheuristic technique used to solve NP-hard problems and find approximate solutions. The term "simulated annealing" is borrowed from the materials science field, where it refers to the process of gradually heating and cooling a material to achieve a desired structure. Similarly, in optimization, this method gradually explores the solution space and approaches the optimal solution (referred to figure 3).

**Figure 3. The concept of simulated annealing**

<img src="/assets/img/box_opt_VDB/SA_concept.png" width="400"/>

The VDB size optimization was also solved using the simulated annealing method, where the values for the base box (three-dimensional size before folding) and the folded height were calculated. The calculation process for applying the simulated annealing method to the VDB size optimization problem is as follows:

- **Initial value setting:**

  - Considering the minimum and maximum box size constraints, randomly generate several combinations of box sizes. (e.g., prepare 26 boxes with random Length/Width/Height such as 20cm/15cm/6cm)

$${lineup}_{k} = \begin{bmatrix}
\left\lbrack {Length}_{a},\ {Width}_{b},\ {Height}_{c} \right\rbrack,\  \\
\left\lbrack {Length}_{d},\ {Width}_{e},\ {Height}_{f} \right\rbrack, \\
 \vdots 
\end{bmatrix}$$

- **Generate neighboring candidate:**

  - Randomly make small changes to the current box size combinations to create new combinations. (e.g., prepare a slightly different set of box sizes, such as 22cm/14cm/4cm)

$${lineup}_{k'} = \begin{bmatrix}
\left\lbrack {Length}_{a} \pm \ \alpha,\ {Width}_{b}\  \pm \ \beta,\ {Height}_{c}\  \pm \gamma \right\rbrack,\  \\
\left\lbrack {Length}_{d} \pm \ \delta,\ {Width}_{e} \pm \ \varepsilon,\ {Height}_{f} \pm \ \epsilon \right\rbrack, \\
 \vdots 
\end{bmatrix}$$

- **Evaluate the lineup:**

  - Evaluate the air rate for the box size combinations and define it as the "energy". The lower the air rate, the lower the energy. (e.g., a box lineup with 49% air rate is considered "better" than one with 50% air rate)

- **Accept temporal solution:**

  - If the air rate decreases, accept the new candidate as the best lineup.

- **Adjust temperature:**

  - Initially, the variation range for generating neighboring candidates is large, but it is gradually reduced over time to approach the optimal solution. (e.g., decrease the Length/Width/Height variation range from 5cm to 3cm)

- **Determine optimal solution:**

  - After executing the above loop for a certain number of iterations, the box lineup with the lowest air rate is adopted as the optimal solution.

The iteration flow is as shown below.

**Figure 4. Simulated Annealing flow diagram**

<img src="/assets/img/box_opt_VDB/SA_execution_flow.png" width="700"/>

This simulated annealing-based approach was used to calculate the optimal base box size before folding and the folded height for the VDB size optimization.  
  
For the VDB size optimization, the simulated annealing method was used to calculate the values for the base box (before folding) and the folded height. Both Case 1 (60-80 girth range) and Case 2 (60-100 girth range) employed the simulated annealing method to find the box lineup with the minimum air rate as the optimal solution. One iteration of the calculation cycle was performed, and 1,000 cycles were run in one job. Since the simulated annealing method is significantly influenced by the randomly generated initial values, 25 parallel jobs were generated, and the job with the lowest air rate was selected as the best solution.  
  
The figure 5 below shows the air rate variation per cycle for the best job calculated in Case 2. The blue dot represents the air rate of the original box lineup traditionally used in the target FC (39.35%), and the green line is the improved air rate after optimization (37.19%). The initial air rate of the random initial value was around 57%, but it decreased and converged as the cycles progressed. Ultimately, the air rate was reduced to 37.19%, a 2.16% improve from the original lineup.

**Figure 5. Optimization curve showing the reduction in air ratio over iteration**

<img src="/assets/img/box_opt_VDB/opt_curve.png" width="700"/>

## **Limitation of Simulated Annealing**

While the simulated annealing method can explore a wide range of the solution space and find the global optimum, it also has some drawbacks. Particularly when using a large dataset, simulated annealing can converge more slowly compared to other optimization algorithms. Additionally, the simulated annealing method heavily depends on the adjustment of parameters such as the initial value, cooling schedule, and stopping conditions. As a result, detailed parameter tuning is required before running the calculations, which can take a significant amount of time in the pre-processing stage. In the case of the size optimization, it took more than 3 weeks to tune the parameters for the "generate neighboring candidates" and "adjust temperature" steps.

Furthermore, the optimal solution and the convergence speed of simulated annealing are highly dependent on the initial values. Since the initial values were set randomly in the current calculation method, the convergence results exhibited large variations. The algorithm randomly modifies the values at each cycle to explore neighboring solutions, and if the efficiency is poor, it may not be able to reach the optimal solution easily. The figure 6 below shows the air rate variation for all 25 jobs, which clearly demonstrates the significant impact of the initial values. The jobs with high initial values or poor solution-finding efficiency resulted in a final air rate exceeding 40%, which is even worse than the original box lineup. When observing the convergence results across all jobs, the final air rate ranged from 40% to 39%.

**Figure 6. Optimization curve showing the reduction in air ratio for all jobs**

<img src="/assets/img/box_opt_VDB/opt_curve_all_job.png" width="700"/>
  
While it is unclear which optimization algorithm is most suitable for the VDB size optimization, it would be desirable to compare the calculation time and accuracy of other optimization algorithms such as genetic algorithm and adopt an algorithm that is specifically tailored to this problem.

## **Balancing Air Rate and Box Variety**

Theoretically, if there were an infinite number of box sizes available, the air rate could be reduced to 0%. However, considering the constraints of on-site pack stations and physical storage space, there is a limit to the number of boxes that can be manufactured. Additionally, as the number of boxes increases, the rate of air rate improvement diminishes, and there exists a "Saturation Point" where adding more boxes does not significantly improve the air rate. The Saturation Point is the point where the benefits of transport cost reduction from air rate improvement are balanced by the increased material costs and lowered productivity.  
The figure 7 below simulates the air rate reduction when the number of boxes is increased infinitely, using 500,000 past annual shipments. The traditional FCs use an average of 25 boxes, with an air rate of around 50%, which aligns with the trend shown in the graph. As the number of boxes is increased to 60-70 or more, the air rate improvement becomes less pronounced, around 45%, and then gradually improves thereafter. Therefore, by replacing the current box lineup with VDB as much as possible and increasing the theoretical number of boxes to around 60-70, effective air rate improvement can be achieved. However, this Saturation Point may not be the optimal target, as the consideration of material costs has not been included in this analysis. Nevertheless, it is clear that increasing the number of boxes beyond this point would have diminishing impact.

**Figure 7. Correspondence between the number of box and air ratio**

<img src="/assets/img/box_opt_VDB/box_ratio_simulation.png" width="1000"/>

## **Appendix**

When determining the optimized VDB configuration, three different calculation approaches were considered to address the question of which existing boxes should be replaced with VDB.  
  
Approach A:

1.  Choose boxes to be replaced which impact least

2.  Define best size of base VDB before folding

3.  Define best folded VDB’s height

<img src="/assets/img/box_opt_VDB/VDB_replacement_flowA.png" width="500"/>

Approach B:

1.  Add optimized VDB on the existing box lineup

2.  Leave out least impact boxes

3.  Define best folded VDB’s height

<img src="/assets/img/box_opt_VDB/VDB_replacement_flowB.png" width="400"/> 
  
Approach C:

1.  Replace the existing box with optimized VDB one by one

2.  Iterate process 1 until introducing all new VDB

3.  Define best folded VDB’s height

<img src="/assets/img/box_opt_VDB/VDB_replacement_flowC.png" width="600"/>
  
Approach B has the potential issue of creating large gaps between the boxes in Step 2, "Leave out least impact boxes". Approach C, which involves replacing VDB one by one, may seem promising, but it could result in getting stuck in local optima by only seeking the optimal solution for each individual step.  
Therefore, for this optimization, Approach A was selected, as it is more likely to yield a global optimum.

Followings are the each dimension of the box lineup and the result of fitting simulation for both Case 1 and 2.


### **Optimized Box lineup for Case 1**

<table style="text-align: center;"><tr><th rowspan="2"><b>ID</b></th><th rowspan="2"><b>Box Name</b></th><th colspan="4"><b>Inner size (cm)</b></th><th colspan="2"><b>Fitting Results</b></th></tr>
<tr><td><b>Girth</b></td><td><b>Length</b></td><td><b>Width</b></td><td><b>Height</b></td><td><b>Air Ratio (%)</b></td><td><b># of fitted ship</b></td></tr>

<tr>
  <td>1</td>
  <td>VDB_7_fold</td>
  <td>42.8</td>
  <td>23.3</td>
  <td>14.5</td>
  <td>5</td>
  <td>75.38</td>
  <td>65,498</td>
</tr>
<tr>
  <td>2</td>
  <td>Existing_box_A</td>
  <td>59.5</td>
  <td>31.2</td>
  <td>22.5</td>
  <td>2.9</td>
  <td>59.91</td>
  <td>7,814</td>
</tr>
<tr>
  <td>3</td>
  <td>VDB_3_fold</td>
  <td>49</td>
  <td>25</td>
  <td>16.9</td>
  <td>7.1</td>
  <td>61.74</td>
  <td>48,000</td>
</tr>
<tr>
  <td>4</td>
  <td>VDB_7</td>
  <td>48.7</td>
  <td>23.3</td>
  <td>14.5</td>
  <td>10.9</td>
  <td>55.65</td>
  <td>30,977</td>
</tr>
<tr>
  <td>5</td>
  <td>VDB_2_fold</td>
  <td>63.1</td>
  <td>38.4</td>
  <td>19.6</td>
  <td>5</td>
  <td>58.19</td>
  <td>7,453</td>
</tr>
<tr>
  <td>6</td>
  <td>VDB_8_fold</td>
  <td>57</td>
  <td>26.7</td>
  <td>21.5</td>
  <td>8.8</td>
  <td>46.85</td>
  <td>29,487</td>
</tr>
<tr>
  <td>7</td>
  <td>VDB_9_fold</td>
  <td>66.5</td>
  <td>42.1</td>
  <td>16.7</td>
  <td>7.7</td>
  <td>58.58</td>
  <td>5,911</td>
</tr>
<tr>
  <td>8</td>
  <td>VDB_5_fold</td>
  <td>67.5</td>
  <td>34.1</td>
  <td>27.3</td>
  <td>6.1</td>
  <td>47.69</td>
  <td>9,242</td>
</tr>
<tr>
  <td>9</td>
  <td>VDB_3</td>
  <td>56.5</td>
  <td>25</td>
  <td>16.9</td>
  <td>14.6</td>
  <td>46.19</td>
  <td>17,164</td>
</tr>
<tr>
  <td>10</td>
  <td>VDB_6_fold</td>
  <td>62.7</td>
  <td>29.8</td>
  <td>23.5</td>
  <td>9.4</td>
  <td>40.48</td>
  <td>14,176</td>
</tr>
<tr>
  <td>11</td>
  <td>VDB_1_fold</td>
  <td>70.4</td>
  <td>37.2</td>
  <td>25.2</td>
  <td>7.9</td>
  <td>38.05</td>
  <td>7,922</td>
</tr>
<tr>
  <td>12</td>
  <td>VDB_2</td>
  <td>68.7</td>
  <td>38.4</td>
  <td>19.6</td>
  <td>10.7</td>
  <td>47.23</td>
  <td>11,306</td>
</tr>
<tr>
  <td>13</td>
  <td>VDB_10_fold</td>
  <td>73.8</td>
  <td>46</td>
  <td>18</td>
  <td>9.8</td>
  <td>48.71</td>
  <td>3,534</td>
</tr>
<tr>
  <td>14</td>
  <td>VDB_4_fold</td>
  <td>67.4</td>
  <td>31.2</td>
  <td>25</td>
  <td>11.1</td>
  <td>33.62</td>
  <td>11,771</td>
</tr>
<tr>
  <td>15</td>
  <td>VDB_8</td>
  <td>64.1</td>
  <td>26.7</td>
  <td>21.5</td>
  <td>15.9</td>
  <td>38.12</td>
  <td>12,894</td>
</tr>
<tr>
  <td>16</td>
  <td>Existing_box_B</td>
  <td>86.3</td>
  <td>45.7</td>
  <td>31.8</td>
  <td>6.4</td>
  <td>55.15</td>
  <td>5,423</td>
</tr>
<tr>
  <td>17</td>
  <td>VDB_9</td>
  <td>73.4</td>
  <td>42.1</td>
  <td>16.7</td>
  <td>14.6</td>
  <td>39</td>
  <td>8,152</td>
</tr>
<tr>
  <td>18</td>
  <td>VDB_5</td>
  <td>73.6</td>
  <td>34.1</td>
  <td>27.3</td>
  <td>12.2</td>
  <td>34.81</td>
  <td>11,928</td>
</tr>
<tr>
  <td>19</td>
  <td>VDB_6</td>
  <td>71.9</td>
  <td>29.8</td>
  <td>23.5</td>
  <td>18.6</td>
  <td>38.05</td>
  <td>14,611</td>
</tr>
<tr>
  <td>20</td>
  <td>VDB_10</td>
  <td>79.8</td>
  <td>46</td>
  <td>18</td>
  <td>15.8</td>
  <td>38.49</td>
  <td>5,012</td>
</tr>
<tr>
  <td>21</td>
  <td>VDB_1</td>
  <td>78.1</td>
  <td>37.2</td>
  <td>25.2</td>
  <td>15.7</td>
  <td>33.08</td>
  <td>16,985</td>
</tr>
<tr>
  <td>22</td>
  <td>Existing_box_C</td>
  <td>99.1</td>
  <td>48.3</td>
  <td>40.7</td>
  <td>7.7</td>
  <td>52.6</td>
  <td>5,925</td>
</tr>
<tr>
  <td>23</td>
  <td>Existing_box_D</td>
  <td>90</td>
  <td>42.6</td>
  <td>34</td>
  <td>11</td>
  <td>37.8</td>
  <td>9,786</td>
</tr>
<tr>
  <td>24</td>
  <td>Existing_box_E</td>
  <td>91.3</td>
  <td>45.7</td>
  <td>31.8</td>
  <td>11.4</td>
  <td>30.87</td>
  <td>4,831</td>
</tr>
<tr>
  <td>25</td>
  <td>VDB_4</td>
  <td>77.9</td>
  <td>31.2</td>
  <td>25</td>
  <td>21.7</td>
  <td>32.32</td>
  <td>9,012</td>
</tr>
<tr>
  <td>26</td>
  <td>Existing_box_F</td>
  <td>86.2</td>
  <td>35.6</td>
  <td>27.9</td>
  <td>20.3</td>
  <td>34.08</td>
  <td>12,014</td>
</tr>
<tr>
  <td>27</td>
  <td>Existing_box_G</td>
  <td>98.9</td>
  <td>50.8</td>
  <td>30.5</td>
  <td>15.2</td>
  <td>39.5</td>
  <td>12,415</td>
</tr>
<tr>
  <td>28</td>
  <td>Existing_box_H</td>
  <td>98.9</td>
  <td>45.7</td>
  <td>35.6</td>
  <td>15.2</td>
  <td>28.51</td>
  <td>14,742</td>
</tr>
<tr>
  <td>29</td>
  <td>Existing_box_I</td>
  <td>98.9</td>
  <td>43.2</td>
  <td>33</td>
  <td>20.3</td>
  <td>32.71</td>
  <td>19,859</td>
</tr>
<tr>
  <td>30</td>
  <td>Existing_box_J</td>
  <td>98.9</td>
  <td>40.6</td>
  <td>30.5</td>
  <td>25.4</td>
  <td>29.66</td>
  <td>9,773</td>
</tr>
<tr>
  <td>31</td>
  <td>Existing_box_K</td>
  <td>98.9</td>
  <td>35.6</td>
  <td>33</td>
  <td>27.9</td>
  <td>19.84</td>
  <td>3,728</td>
</tr>
<tr>
  <td>32</td>
  <td>Existing_box_L</td>
  <td>119.3</td>
  <td>61</td>
  <td>35.6</td>
  <td>20.3</td>
  <td>36.7</td>
  <td>12,964</td>
</tr>
<tr>
  <td>33</td>
  <td>Existing_box_M</td>
  <td>119.2</td>
  <td>53.3</td>
  <td>36.8</td>
  <td>26.7</td>
  <td>43.81</td>
  <td>5,873</td>
</tr>
<tr>
  <td>34</td>
  <td>Existing_box_N</td>
  <td>119.2</td>
  <td>45.7</td>
  <td>38.1</td>
  <td>33</td>
  <td>26.96</td>
  <td>8,075</td>
</tr>
<tr>
  <td>35</td>
  <td>Existing_box_O</td>
  <td>128.2</td>
  <td>48.3</td>
  <td>43.2</td>
  <td>34.3</td>
  <td>28.32</td>
  <td>1,922</td>
</tr>
<tr>
  <td>36</td>
  <td>Existing_box_P</td>
  <td>159.9</td>
  <td>63.5</td>
  <td>48.3</td>
  <td>45.7</td>
  <td>50.33</td>
  <td>4,328</td>
</tr>
<tr><td colspan="6"><b>TOTAL</b></td><td><b>38.65%</b></td><td>447,345</td></tr>
</table>

### **Optimized Box lineup for Case 2**

<table style="text-align: center;"><tr><th rowspan="2"><b>ID</b></th><th rowspan="2"><b>Box Name</b></th><th colspan="4"><b>Inner size (cm)</b></th><th colspan="2"><b>Fitting Results</b></th></tr>
<tr><td><b>Girth</b></td><td><b>Length</b></td><td><b>Width</b></td><td><b>Height</b></td><td><b>Air Ratio (%)</b></td><td><b># of fitted ship</b></td></tr>
<tr>
  <td>1</td>
  <td>VDB_7_fold</td>
  <td>44.8</td>
  <td>30.3</td>
  <td>11.1</td>
  <td>3.4</td>
  <td>77.64</td>
  <td>30,781</td>
</tr>
<tr>
  <td>2</td>
  <td>VDB_14_fold</td>
  <td>43.5</td>
  <td>23.4</td>
  <td>13.9</td>
  <td>6.2</td>
  <td>68.04</td>
  <td>52,416</td>
</tr>
<tr>
  <td>3</td>
  <td>Existing_box_A</td>
  <td>59.5</td>
  <td>31.2</td>
  <td>22.5</td>
  <td>2.9</td>
  <td>58.79</td>
  <td>7,537</td>
</tr>
<tr>
  <td>4</td>
  <td>VDB_16_fold</td>
  <td>50.8</td>
  <td>25.2</td>
  <td>21.5</td>
  <td>4.1</td>
  <td>48.82</td>
  <td>3,995</td>
</tr>
<tr>
  <td>5</td>
  <td>VDB_7</td>
  <td>49.8</td>
  <td>30.3</td>
  <td>11.1</td>
  <td>8.4</td>
  <td>61.55</td>
  <td>24,456</td>
</tr>
<tr>
  <td>6</td>
  <td>VDB_19_fold</td>
  <td>49.6</td>
  <td>26.6</td>
  <td>15.3</td>
  <td>7.6</td>
  <td>46.33</td>
  <td>18,629</td>
</tr>
<tr>
  <td>7</td>
  <td>VDB_14</td>
  <td>49.5</td>
  <td>23.4</td>
  <td>13.9</td>
  <td>12.2</td>
  <td>51.99</td>
  <td>19,467</td>
</tr>
<tr>
  <td>8</td>
  <td>VDB_1_fold</td>
  <td>55.1</td>
  <td>28.5</td>
  <td>18.3</td>
  <td>8.2</td>
  <td>45.61</td>
  <td>18,588</td>
</tr>
<tr>
  <td>9</td>
  <td>VDB_18_fold</td>
  <td>55.4</td>
  <td>26.1</td>
  <td>19.8</td>
  <td>9.4</td>
  <td>39.74</td>
  <td>15,171</td>
</tr>
<tr>
  <td>10</td>
  <td>VDB_16</td>
  <td>57.3</td>
  <td>25.2</td>
  <td>21.5</td>
  <td>10.6</td>
  <td>39.52</td>
  <td>12,750</td>
</tr>
<tr>
  <td>11</td>
  <td>VDB_11_fold</td>
  <td>67.9</td>
  <td>33.2</td>
  <td>28.5</td>
  <td>6.2</td>
  <td>53.03</td>
  <td>12,402</td>
</tr>
<tr>
  <td>12</td>
  <td>VDB_19</td>
  <td>56.8</td>
  <td>26.6</td>
  <td>15.3</td>
  <td>14.9</td>
  <td>41.55</td>
  <td>7,312</td>
</tr>
<tr>
  <td>13</td>
  <td>VDB_5_fold</td>
  <td>74.1</td>
  <td>46.9</td>
  <td>20.4</td>
  <td>6.7</td>
  <td>59.94</td>
  <td>5,406</td>
</tr>
<tr>
  <td>14</td>
  <td>VDB_1</td>
  <td>60.9</td>
  <td>28.5</td>
  <td>18.3</td>
  <td>14.1</td>
  <td>40.96</td>
  <td>9,913</td>
</tr>
<tr>
  <td>15</td>
  <td>VDB_20_fold</td>
  <td>64.8</td>
  <td>34.2</td>
  <td>18.8</td>
  <td>11.7</td>
  <td>46.78</td>
  <td>8,411</td>
</tr>
<tr>
  <td>16</td>
  <td>VDB_8_fold</td>
  <td>72.8</td>
  <td>39.7</td>
  <td>25.5</td>
  <td>7.5</td>
  <td>43.34</td>
  <td>9,569</td>
</tr>
<tr>
  <td>17</td>
  <td>VDB_4_fold</td>
  <td>68.3</td>
  <td>32.2</td>
  <td>26.7</td>
  <td>9.3</td>
  <td>34.5</td>
  <td>8,892</td>
</tr>
<tr>
  <td>18</td>
  <td>VDB_18</td>
  <td>63.8</td>
  <td>26.1</td>
  <td>19.8</td>
  <td>17.9</td>
  <td>38.89</td>
  <td>8,959</td>
</tr>
<tr>
  <td>19</td>
  <td>VDB_6_fold</td>
  <td>75.6</td>
  <td>44.1</td>
  <td>20.6</td>
  <td>10.8</td>
  <td>44.53</td>
  <td>7,061</td>
</tr>
<tr>
  <td>20</td>
  <td>VDB_3_fold</td>
  <td>67.9</td>
  <td>27.9</td>
  <td>25.7</td>
  <td>14.3</td>
  <td>35.1</td>
  <td>9,507</td>
</tr>
<tr>
  <td>21</td>
  <td>VDB_11</td>
  <td>72.9</td>
  <td>33.2</td>
  <td>28.5</td>
  <td>11.2</td>
  <td>31.05</td>
  <td>7,186</td>
</tr>
<tr>
  <td>22</td>
  <td>VDB_20</td>
  <td>69.5</td>
  <td>34.2</td>
  <td>18.8</td>
  <td>16.5</td>
  <td>34.13</td>
  <td>4,749</td>
</tr>
<tr>
  <td>23</td>
  <td>VDB_13_fold</td>
  <td>82.6</td>
  <td>45.2</td>
  <td>29.2</td>
  <td>8.2</td>
  <td>38.45</td>
  <td>7,156</td>
</tr>
<tr>
  <td>24</td>
  <td>VDB_9_fold</td>
  <td>76.8</td>
  <td>39</td>
  <td>24.8</td>
  <td>13</td>
  <td>36.83</td>
  <td>10,852</td>
</tr>
<tr>
  <td>25</td>
  <td>VDB_15_fold</td>
  <td>75</td>
  <td>31</td>
  <td>29.1</td>
  <td>14.8</td>
  <td>32.05</td>
  <td>6,302</td>
</tr>
<tr>
  <td>26</td>
  <td>VDB_5</td>
  <td>82.4</td>
  <td>46.9</td>
  <td>20.4</td>
  <td>15.1</td>
  <td>43.6</td>
  <td>7,593</td>
</tr>
<tr>
  <td>27</td>
  <td>VDB_4</td>
  <td>75.8</td>
  <td>32.2</td>
  <td>26.7</td>
  <td>16.9</td>
  <td>32.28</td>
  <td>8,721</td>
</tr>
<tr>
  <td>28</td>
  <td>VDB_3</td>
  <td>74.8</td>
  <td>27.9</td>
  <td>25.7</td>
  <td>21.2</td>
  <td>34.7</td>
  <td>6,242</td>
</tr>
<tr>
  <td>29</td>
  <td>VDB_8</td>
  <td>80.7</td>
  <td>39.7</td>
  <td>25.5</td>
  <td>15.5</td>
  <td>28.79</td>
  <td>7,736</td>
</tr>
<tr>
  <td>30</td>
  <td>VDB_10_fold</td>
  <td>91.8</td>
  <td>46.5</td>
  <td>35.7</td>
  <td>9.5</td>
  <td>48.87</td>
  <td>10,803</td>
</tr>
<tr>
  <td>31</td>
  <td>VDB_17_fold</td>
  <td>81</td>
  <td>35.1</td>
  <td>31.1</td>
  <td>14.8</td>
  <td>28.98</td>
  <td>3,952</td>
</tr>
<tr>
  <td>32</td>
  <td>VDB_9</td>
  <td>82.2</td>
  <td>39</td>
  <td>24.8</td>
  <td>18.4</td>
  <td>32.48</td>
  <td>4,597</td>
</tr>
<tr>
  <td>33</td>
  <td>VDB_12_fold</td>
  <td>89.9</td>
  <td>44</td>
  <td>33.7</td>
  <td>12.1</td>
  <td>30.34</td>
  <td>6,391</td>
</tr>
<tr>
  <td>34</td>
  <td>VDB_6</td>
  <td>85.2</td>
  <td>44.1</td>
  <td>20.6</td>
  <td>20.5</td>
  <td>31.95</td>
  <td>1,710</td>
</tr>
<tr>
  <td>35</td>
  <td>VDB_13</td>
  <td>90.1</td>
  <td>45.2</td>
  <td>29.2</td>
  <td>15.7</td>
  <td>29.21</td>
  <td>7,786</td>
</tr>
<tr>
  <td>36</td>
  <td>VDB_17</td>
  <td>86.6</td>
  <td>35.1</td>
  <td>31.1</td>
  <td>20.4</td>
  <td>31.75</td>
  <td>10,616</td>
</tr>
<tr>
  <td>37</td>
  <td>VDB_10</td>
  <td>97.5</td>
  <td>46.5</td>
  <td>35.7</td>
  <td>15.3</td>
  <td>28.58</td>
  <td>16,847</td>
</tr>
<tr>
  <td>38</td>
  <td>VDB_15</td>
  <td>89.2</td>
  <td>31</td>
  <td>29.1</td>
  <td>29.1</td>
  <td>33.33</td>
  <td>1,753</td>
</tr>
<tr>
  <td>39</td>
  <td>VDB_2_fold</td>
  <td>94.9</td>
  <td>41.2</td>
  <td>31.3</td>
  <td>22.4</td>
  <td>31.86</td>
  <td>10,183</td>
</tr>
<tr>
  <td>40</td>
  <td>VDB_12</td>
  <td>98</td>
  <td>44</td>
  <td>33.7</td>
  <td>20.3</td>
  <td>25.22</td>
  <td>9,180</td>
</tr>
<tr>
  <td>41</td>
  <td>VDB_2</td>
  <td>100.4</td>
  <td>41.2</td>
  <td>31.3</td>
  <td>27.9</td>
  <td>29.43</td>
  <td>8,489</td>
</tr>
<tr>
  <td>42</td>
  <td>Existing_box_B</td>
  <td>119.3</td>
  <td>61</td>
  <td>35.6</td>
  <td>20.3</td>
  <td>33.88</td>
  <td>10,461</td>
</tr>
<tr>
  <td>43</td>
  <td>Existing_box_C</td>
  <td>119.2</td>
  <td>53.3</td>
  <td>36.8</td>
  <td>26.7</td>
  <td>43.15</td>
  <td>5,373</td>
</tr>
<tr>
  <td>44</td>
  <td>Existing_box_D</td>
  <td>119.2</td>
  <td>45.7</td>
  <td>38.1</td>
  <td>33</td>
  <td>27.71</td>
  <td>8,325</td>
</tr>
<tr>
  <td>45</td>
  <td>Existing_box_E</td>
  <td>128.2</td>
  <td>48.3</td>
  <td>43.2</td>
  <td>34.3</td>
  <td>29.31</td>
  <td>1,954</td>
</tr>
<tr>
  <td>46</td>
  <td>Existing_box_F</td>
  <td>159.9</td>
  <td>63.5</td>
  <td>48.3</td>
  <td>45.7</td>
  <td>50.33</td>
  <td>4,328</td>
</tr>

<tr><td colspan="6"><b>TOTAL</b></td><td><b>37.19%</b></td><td>480,507</td></tr>
</table>

## **Reference**

Simulated Annealing - <https://www.researchgate.net/figure/Principle-of-the-simulated-annealing-algorithm_fig5_360434054>

