<!-- パンくずリスト -->
[top](../index.md) > [Learning Python](./contents.md) > [About Stable Baselines](./about_stable_baselines.md)

### Link

- [Stable Baselines Documentation](https://stable-baselines.readthedocs.io/en/master/)

## Stable Baselines

### Stable Baselines とは

OpenAI によって開発されている, 研究などの目的のために基本的な強化学習のアルゴリズムを提供する [baselines](https://github.com/openai/baselines) という Python ライブラリの安定版.

[baselines](https://github.com/openai/baselines) からアップグレードされた点として,

- 全てのアルゴリズムの構造が統一されている
- コーディング規約として [PEP8](https://pep8-ja.readthedocs.io/ja/latest/) を採用
- 関数とクラスにドキュメントがある
- アルゴリズムの追加: SAC, TD3 (HER support for DQN, DDPG, SAC and TD3)

が挙げられる.

OpenAI によって開発されている gym との相性が良い.

### Quick Example

```python
import gym # 強化学習の環境

from stable_baselines import PPO2 # PPO2 アルゴリズム
from stable_baselines.common.policies import MlpPolicy # 方策が MLP(Multi Layler Perceptron)
from stable_baselines.common.vec_env import DummyVecEnv # 並列プロセス実行環境

env = gym.make('CartPole-v1')
# Optional: PPO2 requires a vectorized environment to run
# the env is now wrapped automatically when passing it to the constructor
# env = DummyVecEnv([lambda: env])

# 強化学習の学習モデルを宣言して学習
model = PPO2(MlpPolicy, env, verbose=1)
model.learn(total_timesteps=10000)

# 賢くなったモデルで実行
obs = env.reset()
for i in range(1000):
    action, _states = model.predict(obs)
    obs, rewards, dones, info = env.step(action)
    env.render()
```
