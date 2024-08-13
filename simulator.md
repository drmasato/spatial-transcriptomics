# 数値シミュレーションを行うためには、細胞間の相互作用をモデル化し、そのダイナミクスを解析する必要があります。
# NetworkXライブラリを用いて細胞間相互作用ネットワークを構築し、細胞間のシグナル伝達のダイナミクスをシミュレートします。

```python
#まず、NetworkXとNumPyをインストールする必要があります。これを行うには、以下のコマンドを実行します。
pip install networkx numpy

```
```python
import networkx as nx
import numpy as np
import matplotlib.pyplot as plt

# 細胞間相互作用ネットワークの構築
G = nx.DiGraph()

# ノード（細胞）の追加
cells = ['Cell1', 'Cell2', 'Cell3', 'Cell4']
G.add_nodes_from(cells)

# エッジ（細胞間相互作用）の追加（例としてランダムに設定）
interactions = [('Cell1', 'Cell2'), ('Cell2', 'Cell3'), ('Cell3', 'Cell4'), ('Cell4', 'Cell1')]
G.add_edges_from(interactions)

# 初期状態の設定（各細胞の活性化状態を0または1で表す）
np.random.seed(42)
initial_states = {cell: np.random.randint(0, 2) for cell in cells}

# シミュレーションパラメータ
num_steps = 10  # シミュレーションステップ数
activation_threshold = 0.5  # 活性化の閾値

# 細胞の状態を保存するための辞書
states = {cell: [] for cell in cells}
for cell, state in initial_states.items():
    states[cell].append(state)

# 数値シミュレーション
current_states = initial_states.copy()
for step in range(num_steps):
    new_states = current_states.copy()
    for cell in cells:
        # 細胞への入力の合計を計算
        input_sum = sum(current_states[neighbor] for neighbor in G.predecessors(cell))
        # 入力の合計が閾値を超えた場合に活性化
        if input_sum > activation_threshold:
            new_states[cell] = 1
        else:
            new_states[cell] = 0
    current_states = new_states.copy()
    for cell, state in current_states.items():
        states[cell].append(state)

# シミュレーション結果のプロット
plt.figure(figsize=(10, 6))
for cell in cells:
    plt.plot(states[cell], label=cell)
plt.xlabel('Time Step')
plt.ylabel('Activation State')
plt.title('Cell Activation Dynamics')
plt.legend()
plt.show()

```
