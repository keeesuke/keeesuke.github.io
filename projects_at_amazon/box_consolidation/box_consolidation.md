<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

## Box Consolidation Overview

\"Box Consolidation Approach\" aimed at deprecating rarely used box types, and distributing the best fit box for each FCs (Fulfillment Centers) from the various box types managed across the entire Amazon Fulfillment Network. The goal of consolidation is to reduce the unnecessary box types in FCs and to choose the best fitting boxes to contribute to package cube reduction. Given the vast number of possible box combinations, a genetic algorithm is employed to swiftly determine the optimal box lineup specialized for FCs in Japan.

In the Amazon network, a total of 400 box lineups are being used, and there are box types that are hardly used at any FCs. Through consolidation approach, our team aims to deprecate unused boxes and select the best pair of boxes in FCs to minimize air ratio. Box sizes are represented by their girth, the sum of the three dimensions, with the girth ranges handled by Amazon Japan.

To verify the effectiveness of the consolidation, the change in air ratio was tested using actual shipment data from a specific FC. This consolidated suite our team selected in FCs improves Cube per Ship by 3.2%, deprecate 8 % unnecessary box, and expects $64,000/month cost savings ($770,000 in annualized savings).


## Problem Statement and Solution

For each new FC launch, new box sizes have been defined, leading to an increase in the number of box types used exclusively within each FC. Due to the lack of standardization in the past, among the 400 total box types, there are redundant boxes of similar sizes. This complexity has complicated inventory management within FCs. Furthermore, FCs operational for several years have seen changes in product mix, with box types not optimized for ASIN Cube.

To address these challenges, I proposed implementing Box Consolidation, which involves the selection and redistribution of boxes across all FCs within the network. This consolidation aims to align box lineups, consolidate rarely used boxes, and halt the production of boxes which are rarely used. We select boxes used throughout the entire network, redistribute these box types to FCs, and optimize them to minimize air rates for each FC. In the figure below, each FC has different box lineup and does not unify with other FCs, which could produce boxes rarely shipped. The decreased number of boxes by consolidation could provide better fitted box and reduce the load for the supplier to manage many boxes.

<img src="/assets/img/box_consolidation/box_consolidation.png" width="800"/>

## Genetic Algorithm

We aim to calculate the combination of boxes that minimizes the air
ratio for Sortable FCs from the 400 types of boxes used across the
network. For instance, if we choose 73 types of boxes used in FCs, the
number of possible combinations is:
$$1.8 \times 10^{18}\ ( \cong \ \frac{400!}{73!\  \times 327!})$$
It is nearly impossible to compute this using a brute force method, so we seek
an approximate optimal solution. This is a typical combinatorial
optimization problem, known as an NP-hard problem, where the goal is to
find a combination that optimizes an evaluation function under various
constraints. We adopted a genetic algorithm with reinforcement learning
elements for this combinatorial optimization. The reasons for choosing a
genetic algorithm are detailed in the appendix.

Genetic Algorithm (GA) is a metaheuristic optimization technique
inspired by the process of natural selection and genetics. It operates
on a population of candidate solutions, evolving them over generations
to find near-optimal solutions to optimization problems. GA consists of
several steps: Initialization, followed by the cycle of Evaluation,
Selection, Cross-Over, Mutation.

- Initialization

Begin with a randomly generated population of candidate solutions, which
is often called individuals. Each individuals represents a list of boxes
LWH (length, width, and height), which is called as gene. The
individuals are composed of 20 possible box lineups, each referred to as
a gene, representing a candidate lineup for implementation in Sortable
FCs.

$$\text{Individual}_{k} = \begin{bmatrix}
\left\lbrack \text{bo}x_{a},\ box_{b},\ box_{c},\ \cdots \right\rbrack, \\
\left\lbrack \text{bo}x_{d},\ box_{e},\ box_{f},\ \cdots \right\rbrack, \\
 \vdots \\
\end{bmatrix}$$

- Evaluation

Evaluate the fitness of each individual in the population. The
evaluation function is based on the air ratio when fitting the
individual boxes to the sizes of products from 500,000 shipments over
the past year in all FCs. The air ratio, calculated using the formula
below, represents the proportion of total air volume in the boxes
relative to the total box volume. A lower air ratio indicates a better
individual. The volume of air is defined as the volume inside the
smallest box that can contain the dimensions of each product, using the
solution size for both single or multi-ship products.

$$
\text{Air Ratio (\%)} = 100 \times \frac{\sum_{k = 1}^{500k} \left( \left[ \text{Fitted Box Volume} \right]_k - \left[ \text{Each Shipment Volume} \right]_k \right)}{\sum_{k = 1}^{500k} \left[ \text{Fitted Box Volume} \right]_k}
$$

- Selection

Select only individuals with an air ratio below the average, making them
candidates for the next generation.

- Cross-Over

Crossover combines genetic information from two parent individuals to
create a new individual with traits inherited from both parents. Genes
from individuals selected after the selection process are exchanged to
form new box lineups and added as new individuals.

- Mutation

Mutation introduces random changes to individuals to maintain genetic
diversity and explore new regions of the solution space. In the usual
mutation process, the value of a gene (box) in a specific individual is
randomly altered. However, in this approach, the logic focuses on
selectively mutating the gene with the highest air ratio. Specifically,
individuals with poor air ratios are deliberately chosen, and one of
their genes (boxes) is swapped with any of the remaining 400 box types,
resulting in an effective mutation logic.

- Execution

After repeating this cycle 200 times, the box lineup with the lowest air
ratio is determined to be the optimal solution.

The execution flow is as follows:

<img src="/assets/img/box_consolidation/execution_flow.png" width="400"/>

## Consolidation Effect Validation

I selected one evaluation FC as the target for verification.

The evaluation method is in the previously mentioned \"Genetic Algorithm
Logic\" section, where actual shipments are assigned into the
most-fitted box lineup, and calculate air ratio by dividing the total
air volume by the total box volume. I calculated the air ratio by
fitting the 500k actual shipments from the evaluation FC into 20 box
lineups.

The results of the PPS verification are as follows.

|                                  | Air Ratio (%) | Middle Mile Trans Cost Saving |
|----------------------------------|---------------|-------------------------------|
| Baseline (existing box lineup)   | 55.20%        | \-                            |
| Consolidation                    | **52.30%**    | **-6.56** JPY/shipment        |

The Air Ratio improved by 2.9% which is equivalent to a transportation cost saving of 6.54 JPY per shipment. When extrapolated to the approximately 305 million box shipments across the entire FCs annually, this translates to a saving of $8.5MM ($1/155 JPY). However, it is important to note that this simulation is based on the annual past shipping data from a specific FC alone, and the actual fitting results may vary due to changes in the sizes of purchased products for upcoming years.
The Box lineup before and after consolidation changed as follows. Although the Air Ratio for some packages worsened, the overall air ratio for all 20 types of packages improved.

<style>
  .centered-table th, .centered-table td {
    text-align: center;
    vertical-align: middle;
  }
</style>

<table cellpadding="5" cellspacing="0" style="text-align: center;" class="centered-table">
    <thead>
        <tr>
            <th colspan="4">Baseline</th>
            <th colspan="4">Consolidation with Genetic Algorithm</th>
        </tr>
        <tr>
            <th>Package Name</th>
            <th>Fitted Shipments</th>
            <th>Air Ratio</th>
            <th>Assigned Air Volume</th>
            <th>Package Name</th>
            <th>Fitted Shipments</th>
            <th>Air Ratio</th>
            <th>Assigned Air Volume</th>
        </tr>
    </thead>
    <tbody>
        <tr>
          <td>Box A</td>
          <td>25,168</td>
          <td>19.42%</td>
          <td>2,442,172</td>
          <td>Box A’</td>
          <td>25,168</td>
          <td>19.42%</td>
          <td>2,442,172</td>
        </tr>
        <tr>
          <td>Box B</td>
          <td>15,413</td>
          <td>52.04%</td>
          <td>2,258,570</td>
          <td>Box B’</td>
          <td>99,969</td>
          <td>26.26%</td>
          <td>18,891,088</td>
        </tr>
        <tr>
          <td>Box C</td>
          <td>135,440</td>
          <td>25.89%</td>
          <td>31,617,779</td>
          <td>Box C’</td>
          <td>19,788</td>
          <td>42.16%</td>
          <td>3,219,889</td>
        </tr>
        <tr>
          <td>Box D</td>
          <td>11,551</td>
          <td>60.19%</td>
          <td>1,686,114</td>
          <td>Box D’</td>
          <td>28,063</td>
          <td>40.28%</td>
          <td>5,228,596</td>
        </tr>
        <tr>
          <td>Box E</td>
          <td>43,027</td>
          <td>46.33%</td>
          <td>12,701,275</td>
          <td>Box E’</td>
          <td>18,967</td>
          <td>55.73%</td>
          <td>3,079,057</td>
        </tr>
        <tr>
          <td>Box F</td>
          <td>66,631</td>
          <td>27.20%</td>
          <td>36,378,351</td>
          <td>Box F’</td>
          <td>26,200</td>
          <td>52.63%</td>
          <td>6,211,360</td>
        </tr>
        <tr>
          <td>Box G</td>
          <td>2,416</td>
          <td>65.38%</td>
          <td>702,663</td>
          <td>Box G’</td>
          <td>12,885</td>
          <td>39.50%</td>
          <td>4,385,224</td>
        </tr>
        <tr>
          <td>Box H</td>
          <td>23,945</td>
          <td>52.62%</td>
          <td>9,734,103</td>
          <td>Box H’</td>
          <td>73,704</td>
          <td>28.89%</td>
          <td>39,309,852</td>
        </tr>
        <tr>
          <td>Box I</td>
          <td>21,667</td>
          <td>43.81%</td>
          <td>12,325,986</td>
          <td>Box I’</td>
          <td>20,223</td>
          <td>53.73%</td>
          <td>7,859,381</td>
        </tr>
        <tr>
          <td>Box J</td>
          <td>34,992</td>
          <td>53.88%</td>
          <td>19,882,724</td>
          <td>Box J’</td>
          <td>17,791</td>
          <td>57.97%</td>
          <td>7,484,659</td>
        </tr>
        <tr>
          <td>Box K</td>
          <td>4,385</td>
          <td>55.07%</td>
          <td>2,979,082</td>
          <td>Box K’</td>
          <td>20,777</td>
          <td>52.76%</td>
          <td>9,938,583</td>
        </tr>
        <tr>
          <td>Box L</td>
          <td>32,662</td>
          <td>50.19%</td>
          <td>28,761,387</td>
          <td>Box L’</td>
          <td>16,429</td>
          <td>61.49%</td>
          <td>7,680,403</td>
        </tr>
        <tr>
          <td>Box M</td>
          <td>5,110</td>
          <td>55.46%</td>
          <td>4,179,189</td>
          <td>Box M’</td>
          <td>24,264</td>
          <td>52.76%</td>
          <td>17,330,900</td>
        </tr>
        <tr>
          <td>Box N</td>
          <td>6,220</td>
          <td>60.29%</td>
          <td>4,944,972</td>
          <td>Box N’</td>
          <td>9,679</td>
          <td>58.32%</td>
          <td>6,717,482</td>
        </tr>
        <tr>
          <td>Box O</td>
          <td>388</td>
          <td>39.42%</td>
          <td>631,858</td>
          <td>Box O’</td>
          <td>8,613</td>
          <td>60.82%</td>
          <td>6,154,179</td>
        </tr>
        <tr>
          <td>Box P</td>
          <td>600</td>
          <td>52.14%</td>
          <td>918,901</td>
          <td>Box P’</td>
          <td>6,362</td>
          <td>60.82%</td>
          <td>4,989,709</td>
        </tr>
        <tr>
          <td>Box Q</td>
          <td>9,133</td>
          <td>44.20%</td>
          <td>17,887,460</td>
          <td>Box Q’</td>
          <td>6,151</td>
          <td>52.04%</td>
          <td>7,929,648</td>
        </tr>
        <tr>
          <td>Box R</td>
          <td>2,648</td>
          <td>60.81%</td>
          <td>4,525,281</td>
          <td>Box R’</td>
          <td>2,505</td>
          <td>56.37%</td>
          <td>3,494,107</td>
        </tr>
        <tr>
          <td>Box S</td>
          <td>1,597</td>
          <td>57.06%</td>
          <td>3,450,531</td>
          <td>Box S’</td>
          <td>959</td>
          <td>59.86%</td>
          <td>1,351,174</td>
        </tr>
        <tr>
          <td>Box T</td>
          <td>1,101</td>
          <td>61.25%</td>
          <td>2,631,555</td>
          <td>Box T’</td>
          <td>2,908</td>
          <td>57.98%</td>
          <td>5,328,728</td>
        </tr>
        <tr>
          <td>Manual Pack</td>
          <td>528</td>
          <td>-</td>
          <td>-</td>
          <td>Manual Pack’</td>
          <td>3,217</td>
          <td>-</td>
          <td>-</td>
        </tr>
        <tr>
          <th>TOTAL</th>
          <td>444,622</td>
          <th>55.20%</th>
          <td>200,639,952</td>
          <td>&nbsp;</td>
          <td>444,622</td>
          <th>52.30%</th>
          <td>169,026,191</td>
        </tr>
    </tbody>
</table>


## **Q&A**

**1. What is VDB, and Why Consolidation Cannot Be Applied to Below 80 Girth Range?**

As part of the Climate Pledge initiative, efforts are underway to reduce CO2 emissions, plastic usage, and costs by improving packaging materials. To support this goal, our team worked to decrease box volume by replacing the current Amazon cardboard boxes with Variable Depth Boxes (VDB). By adjusting the box height to match the packed product’s size, VDBs minimize unused space inside the box, optimize storage efficiency, and expand the variety of box sizes available. VDBs are particularly efficient because a single box can replicate the functionality of two conventional boxes.

Amazon's box types are categorized based on girth ranges: 60cm, 80cm, 100cm, 120cm, and above (e.g. if the girth falls between 60 and 80cm, that box is classified as the 80 girth range), and the girth range where VDB has been introduced in Amazon Japan as of today is only 60 and 80 girth. However, since VDB implementation is underway, this consolidation efforts are focused on girth ranges not covered by VDBs (100 girth~) to avoid collision.

**2. Why is Genetic Algorithm suitable for consolidation?**

There are multiple algorithms available to achieve consolidation. One reason for choosing a genetic algorithm was the potential need to vary the number of target boxes due to operational requirements at FCs or the preferences of box suppliers. This necessitated a method that could efficiently cycle through trial-and-error iterations in a short time frame. Genetic algorithms are well-suited for agile development projects like this one because they can reach optimal solutions quickly without consuming extensive computational resources.
