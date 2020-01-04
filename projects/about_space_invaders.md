<!-- パンくずリスト -->
[top](../index.md) > [Projects](../index.md) > [Space Invaders](./about_space_invaders.md)

### Links

- [OpenAI gym - SpaceInvaders-v0](https://gym.openai.com/envs/SpaceInvaders-v0/)
- [Github - retrobighead/space_invaders](https://github.com/retrobighead/space_invaders)

# Space Invaders



## ベースライン

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
frames = [] # アニメーションフレームの格納用変数

# 1000ステップ分の実行
for step in range(1000):
    # env.render() # 環境を描画

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
    #   info        -- 追加情報等
    # 各種変数については後述...
    observation, reward, done, info = env.step(action)

    if done: # ゲームが終了した時
        print('final step:', step)
        break

# アニメーションの出力
ani = animation.ArtistAnimation(fig, frames, interval=50)
plt.show()
```

## 環境について

OpenAI Gym で提供されている環境については以下を参照.

- [OpenAI Gym Environments](https://gym.openai.com/envs/#atari)
- [Github - openai/gym](https://github.com/openai/gym/blob/master/docs/environments.md#third-party-environments)

### 環境の構築

```python
# 様々な環境が指定可能
env = gym.make('SpaceInvaders-v4')
env = gym.make('CartPole-v1')
env = gym.make('MsPacman-v4')

# 環境一覧の取得
env_ids = [spec.id for spec in gym.envs.registry.all()]
```

### Space Invaders 環境について

Space Invaders のゲームについて, 提供されている環境は12個.

- SpaceInvaders-v0
- SpaceInvaders-v4
- SpaceInvadersDeterministic-v0
- SpaceInvadersDeterministic-v4
- SpaceInvadersNoFrameskip-v0
- SpaceInvadersNoFrameskip-v4
- SpaceInvaders-ram-v0
- SpaceInvaders-ram-v4
- SpaceInvaders-ramDeterministic-v0
- SpaceInvaders-ramDeterministic-v4
- SpaceInvaders-ramNoFrameskip-v0
- SpaceInvaders-ramNoFrameskip-v4

#### v0 / v4

repeat_action_probability の設定値の違い

- v0: 0.25 の割合で入力に関係なく一つ前の行動が繰り返される設定
- v4: 0.00 の割合 (= 一つ前の行動を繰り返す設定なし)

#### ram

- ram 無し: 画面のピクセル値を状態とする
- ram 有り: 画面のピクセル値ではなく, ram(メモリ)の値を状態とする

#### Deterministic / ramNoFrameskip

- NoFrameskip: 行動の繰り返しなし
- (何もなし): (2, 3, 4)の内ランダムな回数分行動を繰り返す
- Deterministic: 必ず4回連続で行動を繰り返す設定

### 基本操作

強化学習においては,
> **agent**(行動主体)が **environment**(環境)に **action**(行動)として働きかけ, その結果として, **observation**(状態)を観測し, **reward**(報酬)を得る.

ことを一つのサイクルとして, 行動の方針を学習していく.

環境について, 主に以下の3つのメソッドを使用する.

```python
# 環境の初期化し, 初期状態を返す
observation = env.reset()

# 環境に対して行動を実行し, 結果を取得
# この結果を使用して, 次の行動を決定する
action = 0
observation, reward, done, info = env.step(action)

# 環境を描画
# 別ウィンドウで描画されるため, 邪魔なときは matplotlib.animation 等を使用
env.render()
```

### 環境に関する情報取得

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

# Space Invaders の場合, 残機の取得が可能
env.unwrapped.ale.lives()
```

### 環境のラッパーについて

gym によって提供されている環境や各種変数はそのラッパーを作成することで, 簡単にカスタマイズができる.

- [Github - openai/gim/docs](https://github.com/openai/gym/blob/master/docs/creating-environments.md)
- [TF 2.0 for Reinforcement Learning - Gym Wrappers](https://alexandervandekleut.github.io/gym-wrappers/)

```python
# 環境の処理を拡張
# gym.Wrapper クラスを継承
class TestWrapper(gym.Wrapper):
  def __init__(self, env):
    super().__init__(self, env)
    self.env = env

  def reset(self, **kwargs):
    self.env.reset(**kwargs)
    # 初期化処理を修正

  def step(self, action):
    observation, reward, done, info = self.env.step(action)
    # 状況, 報酬, 終了判定, 追加情報に追加処理
    return observation, reward, done, info
```

他にも以下の3つのラッパーが提供されている.

- gym.ObservationWrapper
- gym.RewardWrapper
- gym.ActionWrapper

```python
class TestObservationWrapper(gym.ObservationWrapper):
  def __init__(self, env):
    super().__init__(env)

  def observation(self, observation):
    # observation を加工
    return observation
```

```python
class TestRewardWrapper(gym.RewardWrapper):
  def __init__(self, env):
    super().__init__(env)

  def reward(self, reward):
    # reward を加工
    return reward
```

```python
class TestActionWrapper(gym.ActionWrapper):
  def __init__(self, env):
    super().__init__(env)

  def action(self, action):
    # action を加工
    return action
```

### Atari の Wrapper

Atari のゲームを実行するための環境のラッパーが OpenAI によって提供されている.
以下のファイルで提供されているラッパーについて説明する.

- [baselines/atari_wrapper.py](https://github.com/openai/baselines/blob/master/baselines/common/atari_wrappers.py)

また, Space Invaders においては,

- NoopResetEnv
- MaxAndSkipEnv
- EpisodicLifeEnv
- WarpFrame

を使用する.

#### NoopResetEnv

同じ状態から学習がスタートするのを防ぐため, 環境をリセットした時に数フレームだけ何もしない状態を行ってから学習を始める(学習スタートを遅らせる).

```python
noop_max = 30
env = NoopResetEnv(env, noop_max)
# noops = np.random.randint(1, noop_max+1)
# noops フレームだけ何もしない状態を環境リセット時に行う
```

#### FireResetEnv

FIRE を行うまで固定されているゲーム用に, リセット時に自動で FIRE する.

```python
env = FireResetEnv(env)
```

#### EpisodicLifeEnv

Agent が複数ライフを持つ設定のゲームの場合, 1機減る毎に終了判定(done=True)を出力し, ライフが0になった時に本当に環境をリセットする. そうすることで, 多様な初期状態から学習を始められる.

```python
env = EpisodicLifeEnv(env)
```

#### MaxAndSkipEnv

Atari のゲームは60Hzで進行するため, 複数フレームをまとめて同じ行動を入力するようにして学習させる. 数まとめて行動を入力し, 最後の2フレームの内の有効なフレームを状態sとして返すようにする.
(Atari の環境では奇数フレームと偶数フレームで出力が異なる場合があるため, 後ろの2つの画像で最大値を取ることで有効フレームと判定する.)

```python
skip = 4
env = MaxAndSkipEnv(env, 4)
```

#### ClipRewardEnv

報酬を {-1, 0, 1} に正規化する.

```python
env = ClipRewardEnv(env) # 入力は env
```

#### WarpFrame

(210, 160, 3) の状態画像を (84, 84, 1) のグレースケール画像に変換して状態として返す.

```python
env = WarpFrame(env, width=84, height=84, grayscale=True)
```

#### FrameStack

kフレーム分画像をスタックして, reset(), step() の時にkフレーム分の画像を返す.

```python
k = 4
env = FrameStack(env, k)
```

## 実装

[Class Diagram](https://www.draw.io/#G1DmheQ8ubFrXAY8a4eYMdgeaplbKroL9f)
