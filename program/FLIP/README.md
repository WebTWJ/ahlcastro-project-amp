# Fluid Implicit Particle Simulation

This simulation method, which comes from [10 Minute Physics]("https://github.com/matthias-research/pages/blob/master/tenMinutePhysics/18-flip.html"), is a combination of a particle-based and grid-based fluid simulation. It has individual particles, which then carry information to a grid that is then used to update the particles velocity. It uses the grid to handle making the fluid incompressible, by ensuring an equal number of particles leave and enter cells.

The initially defined variables for this are:
- Particles, each containing position and velocity information
- Grid, containing horizontal velocity on vertical cell faces, vertical velocity on horizontal cell faces, and the cell type, either solid fluid or air.
-Constants, such as time, flipRatio, and overRelaxation.

## Step 1:

Apply outside forces to update particle velocity. The formula used for this is:
$$
\vec{v}_{new} = \vec{v}_{old} + \vec{a} \cdot \Delta t
$$
The particles position is then updated based on the new velocity:
$$
\vec{p}_{new} = \vec{p}_{old} + \vec{v}_{new} \cdot \Delta t
$$
For handling collisions with the simulation bounds, it simply clamps particle positions to a minimum value, and sets the velocity component of the bound intercected to zero.

For handling particle collision, first a minimum distance is calculated. Then the distance between all particles in the same cells is calculated. If one is less than the minimum distance, then each is moved by half the overlapping distance.

## Step 2:

The grid is then updated. The simulation has three types of grid cells: air, solid, or fluid. Fluid is if it contains any particles, solid is for borders \ other objects, and air is if no particles are contained within a cell.

The grid velocities are then updated, with the two horizontal sides containing the $\vec{v}_{x}$, and the two verticle sides containing the $\vec{v}_{y}$.

This is calculated by first getting the 'weights' of each particle, which is how close they are too each corner of the cell.

The first step for this is calculating how close the particle is to the bottom left node:
$$
\vec{t}=\frac{\vec{p}_{\text{particle,offset}}-\vec{p}_{\text{bottom-left node}}}{h_{\text{cell}}}
$$
These can then be used to calculated each of the individual weights:
$$
w_{bl}=(1-t_x)*(1-t_y)
$$
$$
w_{br}=t_x*(1-t_y)
$$
$$
w_{tl}=(1-t_x)*t_y
$$
$$
w_{tr}=t_x*t_y
$$
It is important to note that during the calculations the particle position is offset. For calculating the horizontal component of velocity, the particle positions are considered to be vertically offset by half a cell. In the case of vertical component of velocity, the particle positions are offset by half a cell horizontally. These weights should always add up to one.


The velocity for the face of the neighboring side is then calculated as:
$$
\vec{v}_{\text{face}}=\frac{\sum\vec{v}_{\text{particle}}*\text{weight}}{\sum\text{weight}}
$$
It is important to note that the horizontal component of velocity only contributes to the side faces, and the vertical component only contributes to top/bottom faces.

## Step 3:

The fluid is then made incompressible, by calculating the divergence (the difference between the velocity flowing in versus out). Importantly, this is calculation is only run on fluid cells. The openess of a cell, is calculated by:
$$
s=s_{\text{left}}+s_{\text{right}}+s_{\text{top}}+s_{\text{bottom}}
$$
Where the individual contribution from each side is given by the type of cell that side is. For a fluid it is 1, for air it is 1, and for a solid it is 0.

The divergence is then is calculated by:
$$
d=v_{\text{right}}-v_{\text{left}}+v_{\text{top}}-v_{\text{bottom}}
$$
A 'pressure' parameter is then updated based on the divergence, this modifies the grid velocities to having a divergence of zero:
$$
p=\frac{-d}{s}\text{overRelaxation}
$$
The overRelaxation is set at 1.9, and helps to overshoot the pressure needed to solve a given divergence, causing the simulation to reach stability sooner.

This is then used to update the individual velocities of the cell:
$$
v_{\text{left}}-=p*s_{\text{left}}
$$
$$
v_{\text{right}}+=p*s_{\text{right}}
$$
$$
v_{\text{bottom}}-=p*s_{\text{bottom}}
$$
$$
v_{\text{top}}+=p*s_{\text{top}}
$$


## Step 4:

The grid velocities are then transfered back to the particle. This is done using a combination of PIC (Particle in cell) and FLIP. PIC involves only using the surrounding grid nodes to update the new particle velocities. FLIP calculates the particles new velocity by adding the old particle velocity to the change in grid velocity. PIC is more stable, but is undetailed and loses energy quickly. FLIP has more detail but is unstable.

The PIC velocity is calculated by:
$$
\vec{v}_{\text{PIC}}=w_{bl}v_\text{grid,bl}+w_{br}v_\text{grid,br}+w_{tl}v_\text{grid,tl}+w_{tr}v_\text{grid,tr}
$$
The FLIP velocity is calculated by:
$$
\vec{v}_{\text{FLIP}}=\vec{v}_{old}+\Delta\vec{v}_{\text{grid}}
$$
The total new particle velocity is calculated by:
$$
\vec{v}_{\text{new}}=(1-\text{flipRatio})\vec{v}_{\text{PIC}} + (\text{flipRatio})\vec{v}_{\text{FLIP}}
$$

The steps then repeat.
