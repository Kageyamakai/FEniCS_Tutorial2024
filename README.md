# FEniCSの解析の手順
## 本チュートリアルの目的
ここでは， __有限要素法__ (finite element method，FEM)，およびその計算ツールである __FEniCS__ について勉強を行う．まずは，FEniCS及びその関連ツールのセットアップを行い，コードを実行する．その実行結果を __ParaView__ というアプリを用いて可視化し，どのような結果になったかを体験してもらう．

本チュートリアルが終了した地点で以下のことが理解できていることが望ましい．
- FEMとはなにか？どのようなアルゴリズムなのかを理解している．
- これまでの __チュートリアルとの関係性__ を理解している．特に弾性理論（変分原理）で学んだ内容がどこに活きているのかを意識しながらFEniCSのコードを読み解くことが重要．
- AtlasやVScode，ParaViewのセットアップ．それぞれの役割の理解
- 収束計算の種類，それぞれの特色の理解

## 概要
本チュートリアルでは以下の順序に沿って行う．
1. FEMについて学ぶ．
1. [Setupフォルダ](https://github.com/SolidMechanicsGroup/Tutorial2023/tree/main/FEniCS/setup#readme)を開いて各ツールのセットアップを行う．
1. [2年度前のチュートリアル](https://github.com/SolidMechanicsGroup/Fenics_yokota)を開いて順に行ってください．この年はは比較的簡単な解析をいくつか行っています．Windowsのjupyterで順次やっていきましょう．
1. [昨年度のチュートリアル](https://github.com/SolidMechanicsGroup/Tutorial2023/tree/main/FEniCS/tutorial1)を開いてREADMEを読み，1-1のみを行ってください．
1. ParaViewで可視化を行い，実際の変形を見る．
1.[Tutorialフォルダ]を開いて，動的陽解法のコードを実行してみる．
1.Paraviewで可視化を行い，実際の変形を見る．

かなり，難易度が高いため分からないことがあればすぐ周りの人に聞くようにしましょう．また， __指示通りに進んだのにも関わらず，うまくいかなかったところがあった場合__ や， __指示が分かりにくかった場合__ は __Slackで影山にDM__，もしくは直接聞いてください．
## FEMとは？
[昨年度の影山の論文](https://github.com/SolidMechanicsGroup/FEniCS_Tutorial2024/blob/main/FEniCS/FEM.pdf)でFEMの章があるため読んでください．この章は研究室内にある本（有限要素法概説pp39-56）を参考にしました．分かりにくい場合はそちらの方が図も豊富で理解しやすいと思います．


上記を終えてピンと来てない方はFEMのざっくりとしたイメージだけでも掴んで覚えておきましょう．

- 解析の対象となるモデルを細かいレゴブロックのように考え（メッシュ分割），各々のレゴブロックで応力の平衡方程式を解く（弾性理論）．解く際，唯一解の導出が難しいため，近似解として近似関数，重み関数の考え方を導入しながら頑張って変位を導く（FEMのアルゴリズム）．

## 力学モデル構築におけるFEniCSの位置付け

なにか力学モデルを構築する際の手順は以下の順序で行われる．

この流れを理解し， __コードのどの部分が相当する__ か考えながら実行するようにしましょう．
1. 形状モデルの作成（Rhinoceros,Grasshoper）
1. 形状モデルのメッシュ分割(Gmsh)
1. 固体材料の力学条件を偏微分方程式の境界値問題として数理モデル化（弾性理論）
1. 数理モデルから弱形式の応力の平衡方程式を導出(弾性理論)
1. 有限要素空間を作成（FEniCS）
1. 弱形式から連立方程式の算出（FEniCS）
1.  __連立方程式の解を導出__ （FEniCS）
1. 解を可視化し，評価（Paraview）
