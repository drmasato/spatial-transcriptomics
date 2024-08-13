# 細胞間相互作用ネットワークの構築と可視化を行うPythonプログラムを示します。
#このプログラムでは、仮想データを用いて細胞間のシグナル伝達経路、接着分子、サイトカインの発現データを基にネットワークを構築し、可視化を行います。

'''python
pip install networkx matplotlib
'''

'''python
import networkx as nx
import matplotlib.pyplot as plt

# 仮想データ: 細胞間のシグナル伝達経路、接着分子、サイトカインの発現データ
# このデータは例として使用します。実際のデータは実験結果に基づいて取得します。
interactions = [
    ('Cell1', 'Cell2', 'signaling'),
    ('Cell2', 'Cell3', 'adhesion'),
    ('Cell3', 'Cell4', 'cytokine'),
    ('Cell4', 'Cell1', 'signaling'),
    ('Cell1', 'Cell3', 'cytokine'),
    ('Cell2', 'Cell4', 'adhesion')
]

# 細胞間相互作用ネットワークの構築
G = nx.DiGraph()

# ノード（細胞）の追加
cells = set()
for interaction in interactions:
    cells.update([interaction[0], interaction[1]])
G.add_nodes_from(cells)

# エッジ（細胞間相互作用）の追加
for interaction in interactions:
    G.add_edge(interaction[0], interaction[1], interaction_type=interaction[2])

# ネットワークの可視化
pos = nx.spring_layout(G)  # ノード配置の計算

# ノードの描画
nx.draw_networkx_nodes(G, pos, node_size=700, node_color='skyblue')

# エッジの描画
edge_colors = {'signaling': 'blue', 'adhesion': 'green', 'cytokine': 'red'}
for edge in G.edges(data=True):
    nx.draw_networkx_edges(G, pos, edgelist=[(edge[0], edge[1])], edge_color=edge_colors[edge[2]['interaction_type']])

# ノードラベルの描画
nx.draw_networkx_labels(G, pos, font_size=12, font_family='sans-serif')

# エッジラベルの描画
edge_labels = {(edge[0], edge[1]): edge[2]['interaction_type'] for edge in G.edges(data=True)}
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)

# タイトルと凡例の追加
plt.title('Cell-Cell Interaction Network')
blue_patch = plt.Line2D([0], [0], color='blue', lw=2, label='Signaling')
green_patch = plt.Line2D([0], [0], color='green', lw=2, label='Adhesion')
red_patch = plt.Line2D([0], [0], color='red', lw=2, label='Cytokine')
plt.legend(handles=[blue_patch, green_patch, red_patch], loc='best')

# グラフの表示
plt.show()
'''
