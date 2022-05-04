# Federal Reserve Bank of Richmond Office Workspace Reallocation Optimization
SCMA 645 Management Science, 
Virginia Commonwealth University, 
Spring 2020.   
Won Optimization model class competition issued by Dr. J Paul Brooks (Management Science, department chair and professor). Proposed Python, Pyomo and GLPK network optimization model approach with binary variables and logical constraints to simulate reorganization of 1700 workspaces across 17 floors, while allocating for changing project teams and requirements. Provided report to IT Vice President, Christine Holzem at the Federal Reserve Bank of Richmond. ([Report File](https://github.com/bryce-bowles/office-workspace-optimization/raw/main/Federal_Reserve_Bank_Workspace_Final_Report.pdf))


## Summary
 Created an optimization model, reallocating employees, to minimize the number of times an employee will move workspaces and the distance between team members.  We used an “assignment” network optimization problem approach with binary variables and logical constraints. This model can easily be adapted to any floor by updating the information in the spreadsheet. We did this due to the ever-changing environment at the Federal Reserve Banks, whose teams can operate in 9 different states and change projects frequently, creating the need to generate a continuous model for future use.  

The IT functional groups hosted within the Federal Reserve Bank of Richmond reorganize approximately every six months and have asked for help with allocating their office spaces. The task is for us to create an optimization model using the data provided by Vice president of IT, Christine Holzem.  Once we were given the initial assignment information, we met with Christine to clarify some of our assumptions and get additional information. We created a sample model for submission within the first two weeks. After we received the data set for current workers, we had another two weeks to adjust our base model to incorporate the new data and write the final report. 

The optimal solution for the 20 workers requires 11 moves from their current locations and the total distance between members of all teams is 1,230ft. This is broken down into: Team A 110 ft., Team B 550 ft., and Team C 570 ft. It also ensures that the rank of the office space is equivalent with the rank of the worker. We recommend using this optimization model for each floor, whereas you would only need to change the data in the spread sheet. 
 
## Problem:
The Federal Reserve needs to optimize seating locations of 1700 workers on 17 floors, so approximately 100 workers per floor with half teleworking from home. Office spaces are assigned based on rank, and team members need to be in close proximity of one another. The goal is to minimize moves of current workers and minimize distance between team members. A typical floor plan is 110ft x 110ft with four sides (A,B,C,D) and consist of 4 corner offices, 4 3-window offices, 8 2-window offices, 24 exterior cubicles, 24 interior cubicles, 8 interior offices, and two conference rooms (see appendix1, Floorplan). For information on the full assignment, (see appendix 4).

We started with a smaller base model of seven workers having seven offices and decided an “assignment” optimization problem to reassign workers to spaces would be most effective. With this, we began making assumptions for moves and distances (see appendix 2, moves and 3, distance). This allowed us to work out a model that easily adapted to the complete floor plan (see appendix1, Floorplan). The distance spreadsheet is based off the floor plan of how the office spaces are aligned on each side of the building. The moves spreadsheet assigns current offices to the workers but can be updated to address the current office layout. The worker data spreadsheet has current worker data; however, it can be adjusted with actual worker name and information to provide updated results (see appendix 4, worker).

## Data:  
 #### Let: (see [Report File](Federal_Reserve_Bank_Workspace_Final_Report.pdf))
W = {1 - 20} Workers 

S = {ACO, A2W1, A3W, AEC, AIC, A2W2, , B2W1, B3W, BEC, BIC, BIO1, BIO2, B2W2} For the spaces available we assigned letter codes, for instance: (ACO = A side corner office or BEC = B side exterior cubicle)

V = W U S= { 1,2,3……. ACO, A2W1, A2W2}

A = W X S = {(1, ACO), (1, A2W1),……}

D = S∪S

m_ij = move of 〖worker〗_i to 〖space〗_i  i,j∈A

d_jl = distance from 〖space〗_j to 〖space〗_l   j,l∈D

b_i = { 1 if i∈S and -1 if i∈T  

rank_worker = rank of worker

rank_space = rank allowed in space

team_worker = team assignment of worker

capacity = 7 (capacity of office space)

## Objective in words
Assign workers to office spaces so that distance between workers on a team are minimized and the number of workers moving to a new space is minimized, subject to the following constraints:
	* Capacity of space is equal to 1
	* Worker rank is equal to space rank
	* Moves are minimized
	* Distance between team members is minimized
 
## Algebraic Formulation:
Decision Variables:  (see [Report File](Federal_Reserve_Bank_Workspace_Final_Report.pdf))
Let:
xij = 1 if workeri is assigned to spacej , i,j∈A
yijkl = 1 if team memberi  is assigned to spacej and team memberk  is assigned to spacel   , (i,j)∈A,   (k,l)∈A

<img width="443" alt="image" src="https://user-images.githubusercontent.com/65502025/152538699-c35ef837-b632-4bb7-bd1c-1d0c07bc35bc.png">

## Method:
Our optimization model uses an objective function that calculates the total moves based off the spreadsheet “Moves”(see appendix 2) for any worker that has to relocate from their current space and added the total distance based off the spreadsheet “Distance”(see appendix 3) for the distance from 〖space〗_j to 〖space〗_l. We then added the constraint to ensure workers are assigned a space and workspace capacity for cubicles is 7. We only did the cubicle capacity because workspace rank assignment would limit the office assignment on each team. Next, we have a “flip on” constraint for distance that will only add the distance if workers are assigned to the same team. Lastly, we provided a constraint that will only assign spaces that have a rank equivalent to the workers rank. For instance, A side corner office is only assigned to a worker with rank 1. Once setting up the model with Python, we used CoCalc/Anaconda’s Jupyter Lab to run the model and obtained a solution with “glpk” solver.  

## Implementation:
See attached Jupyter Notebook file (appendix 6, Python)
 
## Results:
The following table shows the eleven workers required to move (in blue) and the total distance between team members. Workers 1, 2, 5, 7, 8, 9, 10, 11, 16, 18, and 20 will acquire new workspace locations.  The distance column is the sum of the distances between that individual and the other team members.

<img width="337" alt="image" src="https://user-images.githubusercontent.com/65502025/151858099-e25473c9-1de6-4c4d-a5d7-50aae1beb80a.png">

 
## Conclusion and Recommendations:
While moving a little over half of the workers and a total distance of 1,230 feet may sound like a lot, you will see a great improvement in productivity and efficiency between team members. You could make an assumption that Team 1 will benefit the most out of this move as they attained the least average distance between team members while also having a relatively medium move rate (of 50%) and Team 2 will benefit the least considering 5/6 workers had to move workspaces while obtaining the greatest average distance (91.67ft) between team members.

The strength of this model is the ability to adjust it’s data set to accommodate a greater volume of attributes and values. When implementing new data and increasing the scale, we recommend starting with one floor at a time; creating constraints specific for a floor and then adding complexities if teams need to be divided onto more than one floor. Something to consider are scenarios with exceptions, for example, where a worker is permitted to sit in a workspace outside of their rank. If the model recognizes a worker as currently sitting in a workspace rank higher than allowed, then it will demote them when running the model. Always spot check and troubleshoot for exceptions in the results.  In this data set, worker 5 stands out as exceptionally far away from their teammates, therefore, skewing the average distance for Team 2. For this individual, we would suggest you consider approaching facilities management to see what workspace they could possibly add to make room for this individual to be closer. After all, there may be a possibility we see an increase in people working from home in the future.
 
## Appendix 1: Floorplan
<img width="410" alt="image" src="https://user-images.githubusercontent.com/65502025/151860039-5c614acd-8cab-43af-ae39-44ec8ec278e1.png"> 
 
## Appendix 2 – Moves
<img width="425" alt="image" src="https://user-images.githubusercontent.com/65502025/151860105-69b87636-bda5-48d8-817d-7a9095a9fdea.png">
 
 
## Appendix 3: Distance
<img width="397" alt="image" src="https://user-images.githubusercontent.com/65502025/151860179-c4eb0e68-f4de-4c46-8584-c53b163d37fa.png">

 
 
## Appendix 4: Worker
<img width="229" alt="image" src="https://user-images.githubusercontent.com/65502025/151860227-ad1ed25c-9729-4f20-9389-02cb3f0b5ea3.png">

 
 
