# IBM-AI-Racing-Capstone
IBM SkillsBuild AI Racing League capstone using Python and TORCS

AI driver for the TORCS Corkscrew track, built on the provided `torcs_jm_par.py` example agent.

**Author:** Bhavishka Singh
**Video of fastest lap (Corkscrew, standing start):

## Results

| Version | Best lap on Corkscrew | Damage |
|---|---|---|
| Original code | Did not complete a lap: the car left the road, crashed and stopped moving | Yes |
| My version | [your lap time] | 0 |

## The enhancement

The original driver aims for one fixed target speed everywhere and only brakes lightly when the car is already sliding at a large angle. On Corkscrew it could not slow down for the corners, so it left the road and got stuck.

My version makes the target speed depend on the road ahead. A new function, `corner_speed()`, reads the forward track sensor `S['track'][9]` and lowers the speed limit as the distance to the edge of the track gets shorter. `calculate_throttle()` uses that limit instead of the fixed `TARGET_SPEED`, so the car runs near its full target speed on straights and slows before corners.

I also retuned the driving parameters to make the car stable: gentler steering, stronger centering on the track, and earlier braking.

## Parameters used

```python
TARGET_SPEED = 70
STEER_GAIN = 12
CENTERING_GAIN = 0.35
BRAKE_THRESHOLD = 0.4
GEAR_SPEEDS = [0, 20, 40, 80, 100, 180]
ENABLE_TRACTION_CONTROL = True
```

## How this was made

I used the provided starter code and first tuned its parameters by hand, testing each change in TORCS. I then tried IBM Bob to implement a corner-speed improvement, but its output did not help for this track. I developed the final change myself and tested it on Corkscrew from a standing start.

## What was not changed

- The REQUIRED sections of `torcs_jm_par.py` (the parameter block, `--port` handling and `get_servers_input`)
- The `Client`, `ServerState` and `DriverAction` classes
- The car model and its dynamics
- The livery

## Files

- `torcs_jm_par.py`: the AI driver with the enhancement
- `snakeoil3_gym.py`, `gym_torcs.py`, `practice.xml`: support files from the starter code
- [livery files, with their names]

## How to run

1. Start TORCS and set up the race: Race, Practice, Configure Race, choose Corkscrew and `scr_server`, then click New Race until the blue waiting screen appears.
2. In a terminal inside the container:

```bash
cd /home/student/workspace/torcs/gym_torcs
python3 torcs_jm_par.py
```

The car starts from a standing start and drives the lap.
