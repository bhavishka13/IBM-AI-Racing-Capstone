# IBM-AI-Racing-Capstone
IBM SkillsBuild AI Racing League capstone using Python and TORCS

AI driver for the TORCS Corkscrew track, built on the provided `torcs_jm_par.py` example agent.

**Author:** Bhavishka Singh
**Video of fastest lap (Corkscrew, standing start):** https://youtu.be/EfqOYRFAyTo

## Results

| Version | Best lap on Corkscrew | Damage |
|---|---|---|
| Original code | Did not complete a lap: the car left the road, crashed and stopped moving | Yes |
| My version | 03:09:05 | 0 |

## The enhancement

The original parameters (target speed 100, steer gain 30, brake threshold 0.9) made the car steer too hard and drive too fast for Corkscrew. It left the road, took damage and stopped moving.

My enhancement is a retuned parameter set that keeps the car stable on the whole track:

Lower target speed (100 to 70): the car arrives at corners slower, so it can stay on the road.
Gentler steering (steer gain 30 to 12): at the original gain the steering reached full lock for very small angle errors, which made the car swing from side to side.
Stronger centering (0.20 to 0.35): the car is pulled back towards the middle of the track more firmly, which keeps it away from the edges.

I found these values by changing one parameter at a time and testing each change on the track in TORCS.

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

I used the provided starter code and first tuned its parameters by hand, testing each change in TORCS. I then tried IBM Bob to implement a corner-speed improvement, but its output did not help for this track, it did help with the parameters. I developed the final change myself and tested it on Corkscrew from a standing start.

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
