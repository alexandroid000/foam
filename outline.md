Definitions
-----------

Let a *cell* be defined as an agent with three states, *inflated*, *deflated*,
and *normal*. In the normal state, the agent is a circle with unit volume. In
the inflated state, it has twice its normal volume, and in the deflated state,
it has half the normal volume. *(Note: this constant is arbitrary.)*

Cells occupy an environment that is a network of *tubes*. A tube consists of
two walls, where the shortest distance between the walls is always equal to 
the radius of a cell at normal volume. When a cell inflates, it must then deform
along the direction of the tube. We assume the cells inflate symmetrically, so
they exert equal force to the left and the right of their center before
inflation. Assume that friction is negligible compared to the force of
inflation. *(TODO: work out exact location of equilibrium for any motion
primitives we define)*

Define a *system* as a network of *closed tubes*, meaning that each tube is
connected to either a junction with other tubes or a closed "dead end." The
tubes are of such a length that when all cells in the system are at normal
volume, the entire network of tubes is filled with no empty space between cells
or between cells and a dead end.

In general, we will represent a system as a graph, where nodes in the graph are
junctions between tubes and edges are the tubes themselves.

A system may also contain *objects*, which are static bodies without
any motion capabilities. Let these objects at first be the same size and shape
as a cell in normal state.

Assume time moves in discrete stages. 

*Centralized model:*
At each stage, the state of each cell may be assigned independently.

*Decentralized model:*
At each stage, the state of each cell will be a function of its previous state
and the previous states of its immediate neighbors.

Question: can we do some kind of policy coarsening to turn a centralized
controller into a simple decentralized controller, perhaps with some idea of how
much performance we lose?

Problem Statements
------------------

**Single object movement:** Given a system containing one object at an initial
location $x_i$. Determine a sequence of state changes for each cell that will
transport the object to a given goal $x_f$.

What is the shortest feasible path (distance travelled) for the object? Shortest
path graph search.

What is the path with the fastest completion time? Search for path where the
cumulative length of each cycle along the path is minimal?

When are these solutions not the same solution? (picture example)

What about some other metric? Minimum "energy" (sum of number of non-normal
cells at each stage)? Would be some balance of minimal length path and minimal
cycle number along path.

**Multiple object movement:** Given a system containing $k$ objects at initial
locations $x_{i,1}, \ldots, x_{i,k}$, determine a sequence of state changes for
each cell that will transport the object to a given goal set $x_{f,1}, \ldots,
x_{f,k}$.

Same optimization questions apply here, but now need to ensure feasibility of
all objects moving.

Movement Primitives
-------------------

Let *actions* be sequences of cell state assignments over one or more stages. We
only consider actions that begin and end with all cells assigned to the normal
state, forming a thermodynamic cycle. We will call these types of actions
*closed actions*. (or should we call these "cycle actions" or something else?)

**Proposition:** All closed actions are reversible.

**Proposition:** An object in a tube with $n$ cells between it and the nearest
dead end (with no junctions between the object and the dead end), cannot be
displaced under any closed action. Under non-closed actions, the object can be
displaced at most $n/2$ units in either direction.

**Proposition:** An object in a cyclic system (no junctions) can be moved to any point 
with closed actions.

- **Proof:** Consider clockwise motion. Define an action where the cell to the
  left of the object inflates and the cell to the right of the object deflates.
  This causes a clockwise displacement of the object, but is not a closed
  action. To return the system to normal, we propagate the deflated state
  through the cells in the cycle in the clockwise direction, deflating one at a
  time, until the deflate state is adjacent to the inflated cell, at which point 
  both cells transition to normal state. *(TODO: clean up language, make
  diagram/animation)*.

This movement primitive can be executed with more than one cell
inflating/deflating on either side of the object in one stage, which will lead
to a corresponding larger displacement at each stage.

Movement in Arbitrary Systems
-----------------------------

When considering movement in systems which are not a single edge or a single
cycle, the behavior of cells at tube junctions must be defined.

First assumption: all junctions in the system are 3-valent. Then, at any
junction, the direction of object transport can be defined by only deflating the
cell along the direction of desired transport.

Specifically, if the entire graph is three-valent, then at each junction we must
have either two incoming edges and one outgoing, or one incoming edge and two
outgoing.

When multiple objects are to be moved simultaneously, we must then check for
feasibility by ensuring that homology constraints are satisfied. All assignments
of orientations to all cycles must be compatible.

Dimension of cycle space vs number of objects

Conjecture: if rank of cycle space is equal to or greater than number of
objects, they can be moved arbitrarily.

Toy example: circle with diameter, three objects in middle, what are relations
about how they can be placed?
