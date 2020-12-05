---
layout: post
title:  "Offline Scheduling"
date:   2020-11-26 
categories: case studies
---

Often, in our organization we need to schedule people for certain tasks. 
Let's say if we are setting up a table to recruit new members. 
Currently, we have five officers: Ahmad, Edi, Eva, Fatimah, and Husain. 
Suppose we have booked the space and are planning to have 2 shifts (morning and afternoon) for each day in the week (Monday-Saturday).
To make our table appealing, we want to have at least 2 officers in each shifts, 
but do not want anyone to sit for more than 5 shifts so as to have diversity. We have asked the availability of each officer for the week and summarize it in a table shown below.

<iframe width="800" height="331" frameborder="0" scrolling="no" src="https://onedrive.live.com/embed?resid=D3D972B45C32E866%21290&authkey=%21AKwvJOSqx604lI8&em=2&wdAllowInteractivity=False&Item='offine-scheduling'!C13%3AQ26&wdHideGridlines=True&wdInConfigurator=True"></iframe>

Now, how do we determine the shift assignment while respecting all these constraints? Use our handy friend, optimization modeling!

 We can use integer linear programming model to solve this. Let's use the following notations:

$$
\begin{align}
I :& \text{ Set of officers}\\
J :& \text{ Set of shifts} \\
d_j : & \text{ People at shift } j \in J\\
s_i :& \text{ Number of shifts officer } i \in I \text{ can work} \\
a_{ij} :& \begin{cases}   1, \text{if officer } i \in I \text{ can work at shift } j \in J \\
                        0, \text{else} \end{cases}\\
X_{ij} :& \begin{cases}   1, \text{if officer } i \in I \text{ is assigned to shift } j \in J \\
                        0, \text{else} \end{cases}
\end{align}
$$


For our problem, the set of officers is 

$$I = \{\text{Ahmad, Edi, Eva, Fatimah, Husain}\} $$ 

and the set of shifts is 

$$J = \{ \text{Senin-Pagi, Senin-Siang, Selasa-Pagi, Selasa-Siang}, \cdots, \text{Sabtu-Pagi, Sabtu-Siang}\}$$ 

As shown above, $$s_i = 5, \forall i \in I$$ and  $$d_j = 2, \forall j \in J$$. Next, we have a few constraints. 

Constraint 1: Officers work no more than total available time: 

$$\sum_{j \in J} X_{ij} \leq s_i, \forall i \in I$$


Constraint 2: Each shift is attended by enough officers: 

$$\sum_{i \in I} X_{i,j} \leq d_j, \forall j \in J$$


Constraint 3: Respect officers' availability: 

$$X_{ij} \leq a_{ij}, \forall i \in I; j \in J$$


Constraint 4: Binary decision variables $$X_{ij} \in \{0, 1\}, \forall i \in I; j \in J$$ 


The objective: minimize the number of assignments $$f(X) = \sum_{i \in I} \sum_{j \in J} X_{ij}$$

The blank decision variables look like this.

<iframe width="800" height="243" frameborder="0" scrolling="yes" src="https://onedrive.live.com/embed?resid=D3D972B45C32E866%21290&authkey=%21AKwvJOSqx604lI8&em=2&wdAllowInteractivity=False&Item='offine-scheduling'!C2%3AQ11&wdInConfigurator=True"></iframe>

Solving this problem using standard solver (e.g. Excel's solver or OpenSolver), we obtain the assignment

<iframe width="800" height="252" frameborder="0" scrolling="no" src="https://onedrive.live.com/embed?resid=D3D972B45C32E866%21290&authkey=%21AKwvJOSqx604lI8&em=2&wdAllowInteractivity=False&Item='offine-scheduling'!C63%3AQ72&wdHideGridlines=True&wdInConfigurator=True"></iframe>

You can [download the full worksheet here](https://1drv.ms/x/s!AmboMly0ctnTgiJ9E6ceGuveGffo?e=R1fUi4). 

[To add:] Using [Solver in Excel Online](https://www.solver.com/excel-solver-how-load-or-start-solver)

This is perhaps the simplest model for this problem. You could modify the objective, say to have more consecutive shift assignments, or add constraints to accommodate other features of real life problems. Let me know what problems in your organization you can solve using this offline scheduling formulation!



<div id="data-ketersediaan" style="display:none;"></div>
<script>var dataObject1 = [
  {
    id: 'Ahmad',
    senPag: 1, 
    senMal: 0,
    selPag: 1, 
    selMal: 0,
    rabPag: 1, 
    rabMal: 0,
    kamPag: 1, 
    kamMal: 0,
    jumPag: 1, 
    jumMal: 0,
    sabPag: 0, 
    sabMal: 0
  },
  {
    id: 'Edi',
    senPag: 1, 
    senMal: 0,
    selPag: 1, 
    selMal: 0,
    rabPag: 1, 
    rabMal: 0,
    kamPag: 1, 
    kamMal: 0,
    jumPag: 1, 
    jumMal: 0,
    sabPag: 0, 
    sabMal: 0
  },
  {
    id: 'Eva',
    senPag: 1, 
    senMal: 0,
    selPag: 1, 
    selMal: 0,
    rabPag: 1, 
    rabMal: 0,
    kamPag: 1, 
    kamMal: 0,
    jumPag: 1, 
    jumMal: 0,
    sabPag: 0, 
    sabMal: 0
  },
  {
    id: 'Fatimah',
    senPag: 1, 
    senMal: 0,
    selPag: 1, 
    selMal: 0,
    rabPag: 1, 
    rabMal: 0,
    kamPag: 1, 
    kamMal: 0,
    jumPag: 1, 
    jumMal: 0,
    sabPag: 0, 
    sabMal: 0
  },
  {
    id: 'Husain',
    senPag: 1, 
    senMal: 0,
    selPag: 1, 
    selMal: 0,
    rabPag: 1, 
    rabMal: 0,
    kamPag: 1, 
    kamMal: 0,
    jumPag: 1, 
    jumMal: 0,
    sabPag: 0, 
    sabMal: 0
  }
];


var hotElement1 = document.querySelector('#data-ketersediaan');
var hotElementContainer1 = hotElement1.parentNode;
var hotSettings1 = {
  data: dataObject1,
  columns: [
    {
      data: 'id',
      type: 'text',
      width: 100
    },
    {
      data: 'senPag',
      type: 'numeric'
    },
    {
      data: 'senMal',
      type: 'numeric'
    },
    {
      data: 'selPag',
      type: 'numeric'
    },
    {
      data: 'selMal',
      type: 'numeric'
    },
    {
      data: 'rabPag',
      type: 'numeric'
    },
    {
      data: 'rabMal',
      type: 'numeric'
    },
    {
      data: 'kamPag',
      type: 'numeric'
    },
    {
      data: 'kamMal',
      type: 'numeric'
    },
    {
      data: 'jumPag',
      type: 'numeric'
    },
    {
      data: 'jumMal',
      type: 'numeric'
    },
    {
      data: 'sabPag',
      type: 'numeric'
    },
    {
      data: 'sabMal',
      type: 'numeric'
    }
  ],
  autoWrapRow: true,
  rowHeaders: true,
  colHeaders: [
    'A',
    'B',
    'C',
    'D',
    'E',
    'F',
    'G',
    'H',
    'I',
    'J',
    'K',
    'L',
    'M'
  ],
   licenseKey: 'non-commercial-and-evaluation'
};
var hot1 = new Handsontable(hotElement1, hotSettings1);
</script>

