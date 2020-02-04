

### 環境リスト

- CartPole
- MountainCar


#### CartPole




#### MountainCar

使用アルゴリズムは DDPG.

[Better exploration with parameter noise](https://openai.com/blog/better-exploration-with-parameter-noise/)

```python
import gym
import numpy as np
from stable_baselines.ddpg.policies import MlpPolicy
from stable_baselines.common.vec_env import DummyVecEnv
from stable_baselines.ddpg.noise import NormalActionNoise, OrnsteinUhlenbeckActionNoise, AdaptiveParamNoiseSpec
from stable_baselines import DDPG

import cv2

""" ---------------------------------
    Constants
--------------------------------- """

SAVEFILE = './mountaincar_ddpg.mp4'

""" ---------------------------------
    Main
--------------------------------- """

env = gym.make('MountainCarContinuous-v0')
env = DummyVecEnv([lambda: env])
# the noise objects for DDPG

print('model learning...')

n_actions = env.action_space.shape[-1]
param_noise = None
action_noise = OrnsteinUhlenbeckActionNoise(mean=np.zeros(n_actions), sigma=float(0.5) * np.ones(n_actions))
model = DDPG(MlpPolicy, env, verbose=1, param_noise=param_noise, action_noise=action_noise)
model.learn(total_timesteps=400000)

print('playing...')

frames = []
fig = plt.figure(figsize=(16,9))

obs = env.reset()
for i in range(1000):
    frm = env.render(mode='rgb_array')
    img = cv2.resize(frm, (2048, 1536), interpolation=cv2.INTER_CUBIC)
    frames.append([plt.imshow(img)])

    action, _states = model.predict(obs)
    obs, rewards, dones, info = env.step(action)

env.close()

print('saving animation...')

ani = animation.ArtistAnimation(fig, frames, interval=20)
ani.save(SAVEFILE, writer='ffmpeg')
```
