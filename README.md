
# RoboVat

[About](#about)  
[Installation](#installation)  
[Examples](#examples)  
[Citation](#citation)  

## About

RoboVat is a tookit for fast development of robotic task environments in simulation and the real world. It provides unified APIs for robot control and perception to bridge the reality gap. Its name is derived from [<em>brain in a vat</em>](https://en.wikipedia.org/wiki/Brain_in_a_vat).

Currently, RoboVat supports [Sawyer](https://www.rethinkrobotics.com/sawyer) robot via [Intera SDK](https://github.com/RethinkRobotics/intera_sdk). The simulatied environments run with [PyBullet](https://github.com/bulletphysics/bullet3/). The codebase is under active development and more environments will be included in the future.

<p align="center"><img width="80%" src="docs/push_env.png" /></p>

## Installation

1. **Create a virtual environment (recommended)** 

	Create a new virtual environment in the root directory or anywhere else:
	```bash
	virtualenv --system-site-packages -p python .venv
	```

	Activate the virtual environment every time before you use the package:
	```bash
	source .venv/bin/activate
	```

	And exit the virtual environment when you are done:
	```bash
	deactivate
	```

2. **Install the package** 

  	Using pip to install the package:
	```bash
	pip install robovat
	```

  	The package can also be installed by running:
	```bash
	python setup.py install
	```

3. **Download assets** 

	Download and unzip the assets folder from [Box](https://app.box.com/s/decsiq52cg6w6898ukl60ylmi7gqv2qr) or the FTP link below to the root directory:
	```bash
	wget ftp://cs.stanford.edu/cs/cvgl/robovat/assets.zip
	wget ftp://cs.stanford.edu/cs/cvgl/robovat/configs.zip
	unzip assets.zip
	unzip configs.zip
	```

	If the assets folder is not in the root directory, remember to specify the 
	argument `--assets PATH_TO_ASSETS` when executing the example scripts.

## Examples

### Command Line Interface

A command line interface (CLI) is provided for debugging purposes. We recommend running the CLI to test the simulation environment after installation and data downloading: 
```bash
python tools/sawyer_cli.py --mode sim
```

Detailed usage of the CLI are explained in the source code of `tools/sawyer_cli.py`. The simulated and real-world Sawyer robot can be test using these instructions below in the terminal:
* Visualize the camera images: `v`
* Mouse click and reach: `c`
* Reset the robot: `r`
* Close and open the gripper: `g` and `o`

### Planar Pushing

Execute a planar pushing tasks with a heuristic policy:
```bash
python tools/run_env.py --env PushEnv --policy HeuristicPushPolicy --debug 1
```

To execute semantic pushing tasks, we can add bindings to the configurations:
```bash
python tools/run_env.py --env PushEnv --policy HeuristicPushPolicy --env_config configs/envs/push_env.yaml --policy_config configs/policies/heuristic_push_policy.yaml --config_bindings "{'TASK_NAME':'crossing','LAYOUT_ID':0}" --debug 1
```

To execute the tasks with pretrained [CAVIN](http://pair.stanford.edu/cavin/) planner, please see [this codebase](https://github.com/stanfordvl/cavin).

### Process Objects for Simulation

Many simulators load bodies in the [URDF](http://wiki.ros.org/urdf/XML) format. Given an [OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) file, the corresponding URDF file can be generated by running:
```bash
python tools/convert_obj_to_urdf.py --input PATH_TO_OBJ --output OUTPUT_DIR
```

To simulate concave bodies, the OBJ file needs to be processed by convex decomposition. The URDF file of a concave body can be generated using [V-HACD](https://github.com/kmammou/v-hacd/) for convex decomposition by running:
```bash
python tools/convert_obj_to_urdf.py --input PATH_TO_OBJ --output OUTPUT_DIR --decompose 1
```
