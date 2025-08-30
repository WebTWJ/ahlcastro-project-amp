# Fluid Implicit Particle Simulation

This simulation method, which comes from [10 Minute Physics]("https://github.com/matthias-research/pages/blob/master/tenMinutePhysics/18-flip.html"), is a combination of a particle-based and grid-based fluid simulation. It has individual particles, which then carry information to a grid that is then used to update the particles velocity. It uses the grid to handle making the fluid incompressible, by ensuring an equal number of particles leave and enter cells.

The initially defined variables for this are: BLANK.

## Step 1:

Apply outside forces to update particle velocity. The formula used for this is:
$$
\vec{v}_{new} = \vec{v}_{old} + \vec{a} \cdot \Delta t
$$
The particles position is then updated based on the new velocity:
$$
\vec{p}_{new} = \vec{p}_{old} + \vec{v}_{new} \cdot \Delta t
$$
Then the following operations are used to handle collisions:
$$
BLANK
$$

## Step 2:

The grid is then updated. The simulation has three types of grid cells: air, solid, or fluid. Fluid is if it contains any particles, solid is for borders \ other objects, and air is if no particles are contained within a cell.

The grid velocities are then updated, with the two horizontal sides containing the $\vec{v}_{x}$, and the two verticle sides containing the $\vec{v}_{y}$. This is calculated by combining the velocities of each particle in a cell as such:
$$
BLANK
$$

## Step 3:

The fluid is then made incompressible, by calculating the divergence (the difference between the velocity flowing in versus out). Importantly, this is only enforced where a grid cell doesn't meet air. This allows for bubbles and splashes. This is calculated by:
$$
BLANK
$$
A 'pressure' parameter is then updated based on the divergence, this modifies the grid velocities to having a divergence of zero:
$$
BLANK
$$

## Step 4:

The grid velocities are then transfered back to the particle. This is done using a combination of PIC (Particle in cell) and FLIP. PIC involves only using the surrounding grid nodes to update the new particle velocities. FLIP calculates the particles new velocity by adding the old particle velocity to the change in grid velocity. PIC is more stable, but is undetailed and loses energy quickly. FLIP has more detail but is unstable.

The PIC velocity is calculated by:
$$
BLANK
$$
The FLIP velocity is calculated by:
$$
BLANK
$$
The total new particle velocity is calculated by:
$$
\vec{v}_{new}=(1-flipRatio)\vec{v}_{PIC} + (flipRatio)\vec{v}_{FLIP}
$$

The steps then repeat.
