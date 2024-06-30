## Box Consolidation Overview

\"Box Consolidation Approach\" aimed at selecting and implementing the
most suitable boxes for FCs (Fulfillment Centers) from the variety of
box types managed across the entire Amazon Fulfillment Network. The goal
of consolidation is to reduce the unnecessary box types in FCs and to
choose the best fitting boxes to contribute to package cube reduction.
Given the vast number of possible box combinations, a genetic algorithm
is employed to swiftly determine the optimal box lineup specialized for
Sortable FCs.

In the Amazon network, a total of 400 box lineups are being used, and
there are box types that are hardly used at any FCs. Through
consolidation approach, our team was going to deprecate unused boxes and
select the best pair of boxes in FCs to minimize air ratio. Box sizes
are represented by their girth, the sum of the three dimensions, with
the girth ranges handled by Amazon Japan.

To verify the effectiveness of the consolidation, the change in air
ratio was tested using actual shipment data from one FC. This
consolidated suite our team selected in FCs improves Cube per Ship by
3.2%, deprecate 8 % unnecessary box, and expects 6.4MM JPY/month cost
savings (77.2M JPY in annualized savings).

## Problem Statement and Box Consolidation Approach

For each new FC launch, new box sizes have been defined, leading to an
increase in the number of box types used exclusively within each FC. Due
to the lack of standardization in the past, among the 400 total box
types, there are redundant boxes of similar sizes. This complexity has
complicated inventory management within FCs. Furthermore, FCs
operational for several years have seen changes in product mix, with box
types not optimized for ASIN Cube.

To address these challenges, I proposed implementing Box Consolidation,
which involves the selection and redistribution of boxes across all FCs
within the network. This consolidation aims to align box lineups,
consolidate rarely used boxes, and halt the production of boxes which
are rarely used. We select boxes used throughout the entire network,
redistribute these box types to FCs, and optimize them to minimize air
rates for each FC. In the figure below, each FC has different box lineup
and does not unify with other FCs, which could produce boxes rarely
shipped. The decreased number of boxes by consolidation could provide
better fitted box and reduce the load for the supplier to manage many
boxes.

![box_consolidation image](/assets/img/box_consolidation.png)

## Genetic Algorithm Logic

We aim to calculate the combination of boxes that minimizes the air
ratio for Sortable FCs from the 400 types of boxes used across the
network. For instance, if we choose 73 types of boxes used in FCs, the
number of possible combinations is
$1.8 \times 10^{18}\ ( \cong \ \frac{400!}{73!\  \times 327!})$. It is
nearly impossible to compute this using a brute force method, so we seek
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

$$Air\ Ratio\ (\%) = 100\  \times \ \frac{\sum_{}^{}{{(\left\lbrack \text{Fitted\ Box\ Volume} \right\rbrack}_{k} - \ \left\lbrack \text{Each\ Shipment\ Volume} \right\rbrack_{k})}}{\sum_{}^{}\left\lbrack \text{Fitted\ Box\ Volume} \right\rbrack_{k}}$$

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

## Consolidation Effect Validation

I selected one evaluation FC as the target for verification.

The evaluation method is in the previously mentioned \"Genetic Algorithm
Logic\" section, where actual shipments are assigned into the
most-fitted box lineup, and calculate air ratio by dividing the total
air volume by the total box volume. I calculated the air ratio by
fitting the 500k actual shipments from the evaluation FC into 20 box
lineups.

The results of the PPS verification are as follows.

                                   Air Ratio (%)   Middle Mile Trans Cost Saving
  -------------------------------- --------------- -------------------------------
  Baseline (existing box lineup)   55.20%          \-
  Consolidation                    **52.30%**      **-6.56** JPY/shipment

The Air Ratio improved by 2.9% which is equivalent to a trans cost
saving of 6.54 JPY per shipment.

The Box lineup before and after consolidation changed as follows.
Although the Air Ratio for some packages worsened, the overall air ratio
for all 20 types of packages improved.

|                                  | Air Ratio (%) | Middle Mile Trans Cost Saving |
|----------------------------------|---------------|-------------------------------|
| Baseline (existing box lineup)   | 55.20%        | \-                            |
| Consolidation                    | **52.30%**    | **-6.56** JPY/shipment        |

The Air Ratio improved by 2.9% which is equivalent to a trans cost saving of 6.54 JPY per shipment.

The Box lineup before and after consolidation changed as follows. Although the Air Ratio for some packages worsened, the overall air ratio for all 20 types of packages improved.


|Baseline||||| Consolidation with Genetic Algorithm|||||
|-----------------------------------|------------------|-------------------|---------------------|----------------|--------------------------------------|-----------------|------------------|---------------------|----------------|
| **Package Name**                  | **Fitted Shipments** | **Air Ratio** | **Assigned Air Volume** | **Package Volume** | **Package Name**                         | **Fitted Shipments** | **Air Ratio** | **Assigned Air Volume** | **Package Volume** |
| Package A      | 25,168           | 19.42%     | 2,442,172           | 3,030,655      | 06S04        | 25,168           | 19.42%    | 2,442,172            | 3,030,655      |
| Package B      | 15,413           | 52.04%     | 2,258,570           | 4,709,442      | 06S08        | 99,969           | 26.26%    | 18,891,088           | 25,617,656     |
| Package C      | 135,440          | 25.89%     | 31,617,779          | 42,663,600     | 06S09        | 19,788           | 42.16%    | 3,219,889            | 5,566,938      |
| Package D      | 11,551           | 60.19%     | 1,686,114           | 4,235,290      | 06N11        | 28,063           | 40.28%    | 5,228,596            | 8,755,656      |
| Package E      | 43,027           | 46.33%     | 12,701,275          | 23,664,420     | 07S09        | 18,967           | 55.73%    | 3,079,057            | 6,954,440      |
| Package F      | 66,631           | 27.20%     | 36,378,351          | 49,973,250     | 07S11        | 26,200           | 52.63%    | 6,211,360            | 13,113,624     |
| Package G      | 2,416            | 65.38%     | 702,663             | 2,029,440      | 09S08        | 12,885           | 39.50%    | 4,385,224            | 7,247,813      |
| Package H      | 23,945           | 52.62%     | 9,734,103           | 20,544,810     | 08N14        | 73,704           | 28.89%    | 39,309,852           | 55,278,000     |
| Package I      | 21,667           | 43.81%     | 12,325,986          | 21,937,838     | 08S16        | 20,223           | 53.73%    | 7,859,381            | 16,987,320     |
| Package J      | 34,992           | 53.88%     | 19,882,724          | 43,110,144     | 08S21        | 17,791           | 57.97%    | 7,484,659            | 17,809,503     |
| Package K      | 4,385            | 55.07%     | 2,979,082           | 6,630,120      | 10N13        | 20,777           | 52.76%    | 9,938,583            | 21,036,713     |
| Package L      | 32,662           | 50.19%     | 28,761,387          | 57,746,416     | FY65         | 16,429           | 61.49%    | 7,680,403            | 19,946,449     |
| Package M      | 5,110            | 55.46%     | 4,179,189           | 9,381,960      | 10S16        | 24,264           | 52.76%    | 17,330,900           | 36,687,168     |
| Package N      | 6,220            | 60.29%     | 4,944,972           | 12,452,440     | 10S20        | 9,679            | 58.32%    | 6,717,482            | 16,115,535     |
| Package O      | 388              | 39.42%     | 631,858             | 1,042,944      | 10S22        | 8,613            | 60.82%    | 6,154,179            | 15,705,564     |
| Package P      | 600              | 52.14%     | 918,901             | 1,920,000      | 10Q29        | 6,362            | 60.82%    | 4,989,709            | 12,736,724     |
| Package Q      | 9,133            | 44.20%     | 17,887,460          | 32,056,830     | 12S22        | 6,151            | 52.04%    | 7,929,648            | 16,533,888     |
| Package R      | 2,648            | 60.81%     | 4,525,281           | 11,546,604     | 12S28        | 2,505            | 56.37%    | 3,494,107            | 8,009,111      |
| Package S      | 1,597            | 57.06%     | 3,450,531           | 8,034,906      | 12Q34        | 959              | 59.86%    | 1,351,174            | 3,366,090      |
| Package T      | 1,101            | 61.25%     | 2,631,555           | 6,791,794      | 13Q36        | 2,908            | 57.98%    | 5,328,728            | 12,680,334     |
| Exception      | 528              | \-         | \-                  | \-             | Manual Pack  | 3,217            | \-        | \-                   | \-             |
| **TOTAL**      | 444,622          | **55.20%**  | 200,639,952         | 363,502,902    |              | 444,622          | **52.30%**| 169,026,191          | 323,179,181    |


## Appendix: Why is Genetic Algorithm suitable for consolidation?

There are multiple algorithms available to achieve consolidation. For
this project, I chose to adopt a genetic algorithm. One reason for this
choice was the potential need to vary the number of target boxes due to
operational requirements at FCs or the preferences of box suppliers.
This necessitated a method that could efficiently cycle through
trial-and-error iterations in a short time frame. Genetic algorithms are
well-suited for agile development projects like this one because they
can reach optimal solutions quickly without consuming extensive
computational resources.

There are several mathematical algorithms for combinatorial optimization
problems. Below is a summary of the pros and cons of each:

| Algorithm          | Pro                                                                 | Con                                                              | Box Consolidation Adaptability                                                     |
|--------------------|---------------------------------------------------------------------|------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Genetic Algorithm  | Capable of exploring a diverse range of candidate solutions, making it less prone to getting stuck in local optima. | Random search can be inefficient because it does not learn to explore towards optimal solution. | - Can improve search efficiency by guiding the random search towards lower air ratios, even though it primarily relies on random exploration.<br>- Can achieve optimal solutions quickly without requiring extensive computational resources. |
| Dynamic Programming| Guarantees an optimal solution and is efficient when the computational complexity is low. | As the problem size increases, the computation time grows exponentially. | Given the vast number of box combinations mentioned earlier, it would take too long to compute the optimal solution. |
| Depth-First/Breadth-First Search | Capable of exploring the entire solution space, ensuring that the optimal solution can be found if it exists. | Computation time increases exponentially, and memory usage can be very high when the search space is vast. | Given the vast number of box combinations mentioned earlier, significant computation time and resources are required. |
| Constraint Programming Solver | Handles discrete decision variables and complex constraints effectively, and supports a wide range of constraint types. | Performance heavily depends on the problem's formulation and constraint propagation, and may require manual tuning of search strategies. | The pre-processing of manual parameter settings can be complex, and the computation time can be long. |
