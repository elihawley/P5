# Names
- Elia Hawley
- Martin Uribe Hernandez

## Participating in Competition?
- No

## Levels as Grid
- Selection
    - Chose tournament selection, then roulette wheel selection, because we wanted the algorithm to move towards fitter children while keeping a decent amount of randomness for diversity/exploration. We didn't want a deterministic selection strategy because that would lead us towards local maximum and we did not want a totally random strategy either.

- Initialization Improvements
    - Only allow blocking_options tiles if above and below neighbors are non-blocking
    - Give lower weight to enemies to not have too many
    - Only allow perpendicular neighbors of pipe segments to be pipe segments or tops

- Fitness Function Improvements
    - Added penalty structure from DE
    - Penalties
        - Any coin or enemy on the bottom-most level
        - Enemies that don't start on ground

- Mutation
    - We have a default MUTATION_RATE that may be weighted differently depending on the tile_type so that it is not uniformly random and will skew away from certain types of tiles.
    - We preferred having less empty space, fewer walls, and more interactive tiles.
    - Added constraints include:
        - Only allow blocking_options tiles if above and below neighbors are non-blocking
        - Give lower weight to enemies to not have too many
        - Only allow perpendicular neighbors of pipe segments to be pipe segments or tops


- Crossover
    - Produce 1 child at a time given 2 random parents to promote diversity/exploration. Since the parents are random, left genome vs right genome doesn't matter.
    - We use single-point crossover for simplicity. It's also difficult to make the uniform crossover produce results that don't look scrambled. Multi-point crossover also makes the grid crossovers more visible to users, which looks less nice.
    - Added constraints include:
        - Only allow perpendicular neighbors of pipe segments to be pipe segments or tops

- Favorite Level (Grid)
    - Why do we like it?
        - Balances difficulty, rewards for players, dead ends, enemy spawn placement, use of workaround paths
        - The player is forced to be creative from the beginning steps. To make significant progress, they would need to think (or get) out of the box.
    - How many generations did it take?
        - 22
    - How many seconds did it take?
        - 1007

## Levels as DEs
- How does the mutation in mutate() work?
    - 10% chance for mutation
    - Gets random DE
    - Depending on DE type:
        - enemy => does nothing (i.e. keeps enemies where they are)
        - block => 33% chance to offset x, 33% chance to offset y, 34% chance to switch breakable
        - qblock => same as for block, but switch has_powerup
        - coin => 50% chance to offset x, 50% chance to offset y
        - pipe => 50% chance to offset x, 50% chance to offset height of pipe
        - hole => 50% chance to offset x, 50% chance to offset width of hole
        - stairs => 33% chance to offset x, 33% chance to offset y, 34% chance to switch direction of stairs
        - platform => 25% chance to offset x, 25% chance to offset y, 25% chance to offset width, 25% chance to switch the type of blocks it is made up of

- How does the crossover in generate_children() work?
    - The variable point crossover looks like a variation of a multiple-point crossover. It takes a random point for the left genome and a potentially different random point for the right genome, then swaps the "halves" of each genome.
    - e.g. 
        L      R           L      R
        L      R           L      R
       ---     R    ==>    R      R
        L      R                  R
        L     ---                 L
        L      R                  L
                                  L
    - This produces diverse levels with different DEs and different numbers of DEs because not only are the DEs themselves being crossed over for diversification but also their quantities. This is combined with the fact that the genome is a heapq of DEs sorted by X, then type, etc, which basically allows the algorithm to swap entire sections of levels in each crossover.

- Initialization Improvements
    - Scaled height of pipes and right-stairs by x to discourage unscalable walls
    - Scaled platform height based on count of platforms or stairs under it to encourage reachability

- Fitness Function Improvements
    - Changed penalties to a more general adjustment
    - Encourage more coins
    - Discouraged too many holes + large hole widths

- Mutation Improvements
    - Scaled height mutations of pipes by x to discourage unscalable walls
    - Scaled height mutations of right-stairs and flip mutations of left-stairs by x to discourage unscalable walls

- Favorite Level (DE)
    - Why do we like it?
        - Clean and a good start, actually has some interesting aesthetic landmarks and rewarding gameplay structures, relatively easy, doesn't look totally random
    - How many generations did it take?
        - 23
    - How many seconds did it take?
        - 627

