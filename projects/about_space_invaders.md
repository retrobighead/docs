<!-- パンくずリスト -->
[top](../index.md) > [Projects](../index.md) > [Space Invaders](./about_space_invaders.md)

### Links

- [OpenAI gym - SpaceInvaders-v0](https://gym.openai.com/envs/SpaceInvaders-v0/)
- [Github - retrobighead/space_invaders](https://github.com/retrobighead/space_invaders)

## Space Invaders



### ベースライン

```python
import gym
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# 環境の構築
env = gym.make('SpaceInvaders-v4')

# 環境情報の確認
print('Observation Space: ', env.observation_space)
print('Action Space: ', env.action_space)
print('Action meanings: ', env.unwrapped.get_action_meanings())

# 環境の初期化
observation = env.reset()

fig = plt.figure() # アニメーション実行用
frames = []　# アニメーションフレームの格納用変数

# 1000ステップ分の実行
for step in range(1000):
    # アニメーションフレームの格納
    frame = plt.imshow(observation)
    frames.append([frame])

    # ここで, 何かしらのアルゴリズム (主に強化学習) を用いて
    # obvervation から action を決定する
    action = env.action_space.sample()  # ランダムに選択

    # 環境に対して action を実行
    #   observation -- 観測される状態
    #   reward      -- 報酬
    #   done        -- 終了判定
    #   info        -- デバッグ情報等
    # 各種変数については後述...
    observation, reward, done, info = env.step(action)

    if done: # ゲームが終了した時
        print('final step:', step)
        break

# アニメーションの出力
ani = animation.ArtistAnimation(fig, frames, interval=50)
plt.show()
```

### 環境について

OpenAI Gym で提供されている環境については以下を参照.

- [OpenAI Gym Environments](https://gym.openai.com/envs/#atari)
- [Github - openai/gym](https://github.com/openai/gym/blob/master/docs/environments.md#third-party-environments)

#### 環境の構築

```python
# 様々な環境が指定可能
env = gym.make('SpaceInvaders-v4')
env = gym.make('CartPole-v1')
env = gym.make('MsPacman-v4')

# 環境一覧の取得
env_ids = [spec.id for spec in gym.envs.registry.all()]
```

#### 環境に関する情報取得

強化学習においては,
> **agent**(行動主体)が **environment**(環境)に **action**(行動)として働きかけ, その結果として, **observation**(状態)を観測し, **reward**(報酬)を得る.

ことを一つのサイクルとして, 行動の方針を学習していく.

環境に関する各種変数についての情報を取得する方法を以下に示す.

```python
# 行動の空間を取得
env.action_space # => Discrete(6)
# Discrete space: 非負の整数の空間
env.unwrapped.get_action_meanings()
# => ['NOOP', 'FIRE', 'RIGHT', 'LEFT', 'RIGHTFIRE', 'LEFTFIRE']

# 状態の空間を取得
env.observation_space
# => Box(210, 160, 3)
# Box space: n次元の空間

# 状態の上限と下限を取得
env.observation_space.high # 各要素に対する最大値
env.observation_space.low  # 各要素に対する最小値

# 報酬の上限と下限を取得
env.reward_range

# Space Invaders の場合, 残機の取得が必要
env.unwrapped.ale.lives()
```

#### 環境の起動 / 実行