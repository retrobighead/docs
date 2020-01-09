<!-- パンくずリスト -->
[top](../index.md) > [Learning Python](./contents.md) > [About Stable Baselines](./about_stable_baselines.md)

### Link

- [Stable Baselines Documentation](https://stable-baselines.readthedocs.io/en/master/)
- [Github - RL Tutorial](https://github.com/araffin/rl-tutorial-jnrr19)
- [Github - RL Baselines Zoo](https://github.com/araffin/rl-baselines-zoo)

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

### 強化学習について

- [Reinforcement Learning Tips and Tricks](https://stable-baselines.readthedocs.io/en/master/guide/rl_tips.html)

#### おすすめの学習方法

1. [Resource Page](https://stable-baselines.readthedocs.io/en/master/guide/rl.html) を読んで強化学習を学ぶ
1. [Stable Baselines](https://stable-baselines.readthedocs.io/en/master/) を読んで, [RL Tutorial Colab](https://github.com/araffin/rl-tutorial-jnrr19) をやってみる

強化学習はデータセットが用意されておらず, エージェントが環境と相互作用しながらデータを収集するため, データの質は低くなりやすい。そのため, 環境の小さな違い (seed値の違い等) によって, 結果が大きく違うことがあり, いくつかの別の環境での実行と定量評価が必要。

また, 適切なハイパーパラメータを選択することも必要。デフォルト値に期待するな。[RL Baselines Zoo](https://github.com/araffin/rl-baselines-zoo) にチューニングされたエージェントが例示されており, 参考にするべし。

#### 強化学習の限界
