## Installation and Configuration of MuJoCo, Gym, Baselines

Ubuntu 20.04.3 LTS \
Anaconda 4.10.3 \
Nvidia driver: 470.82.00 \
CUDA Version: 11.4

### 1. MuJoCo installation
Gym version 0.21.0 has installation problems with MuJoCo version 2.1.0, so version 1.5.0 is installed instead: 
  * Download [MuJoCo](https://roboti.us/download/mjpro150_linux.zip), and [licence key](https://roboti.us/file/mjkey.txt)
    * `mkdir ~/.mujoco && cd ~/.mujoco`
    * `unzip mjpro150_linux.zip ~/.mujoco`
    * `mv mjkey.txt ~/.mujoco`
    * `nano ~/.bashrc`
  * Add the following two lines:
  ```
    export LD_LIBRARY_PATH=/home/${USER}/.mujoco/mujoco150/bin${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export MUJOCO_KEY_PATH=/home/${USER}/.mujoco${MUJOCO_KEY_PATH}
  ```  
  * Restart terminal
    * `source ~/.bashrc`
  * Test MuJoCo
    * `cd ~/.mujoco/mujoco150/bin`
    * `./simulate ../model/humanoid.xml`
    
### 1.2. Install mujoco-py
  * Create enviroment    
    * `conda create -n mujoco-gym python=3.9`
    * `conda activate mujoco-gym`
    * `pip3 install -U 'mujoco-py<1.50.2,>=1.50.1'`
    * `python3`
  
  * Test mujoco-py installation by creating a python script:
    * `gedit test-mujoco-py`
  
  ```
  import mujoco_py
  import os
  mj_path, _ = mujoco_py.utils.discover_mujoco()
  xml_path = os.path.join(mj_path, 'model', 'humanoid.xml')
  model = mujoco_py.load_model_from_path(xml_path)
  sim = mujoco_py.MjSim(model)

  print(sim.data.qpos)
  sim.step()
  print(sim.data.qpos)
  ```
  
### 2. Gym installation
  * Install all dependecies
    * `pip install gym[all]`
  * Test gym installation by creating a python script:
    * `gedit test-gym`
  
  ```
  import gym
  env = gym.make('Humanoid-v2')

  from gym import envs
  print(envs.registry.all())    # print the available environments

  print(env.action_space)
  print(env.observation_space)
  print(env.observation_space.high)
  print(env.observation_space.low)

  for i_episode in range(200):
      observation = env.reset()
      for t in range(100):
          env.render()
          print(observation)
          action = env.action_space.sample()    # take a random action
          observation, reward, done, info = env.step(action)
          if done:
              print("Episode finished after {} timesteps".format(t+1))
              break
  env.close()
  ```
  
  * If this error appears when running the script:
    * `ERROR: GLEW initalization error: Missing GL version`
  * Install the following package:
    * `sudo apt install libglew-dev`
  * And add the following line to `~/.bashrc`
    * `export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libGLEW.so`
  * Restart terminal:
    * `source ~/.bashrc`
  * And activate enviroment:
    * `conda activate mujoco-gym`
  
### 3. Installation of stable-baselines3
  * Fist install some required packages:
    * `sudo apt update && sudo apt install cmake libopenmpi-dev python3-dev zlib1g-dev`
  * Install Pytorch
    * `conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch`
  * And then, baseline3
    * `pip install stable-baselines3[extra]`
  
  
