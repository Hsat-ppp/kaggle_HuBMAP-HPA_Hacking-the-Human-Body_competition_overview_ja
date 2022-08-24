# kaggle_HuBMAP-HPA_Hacking-the-Human-Body_competition_overview_ja
Kaggle competition overview for "HuBMAP + HPA - Hacking the Human Body." <br>
Competition page: https://www.kaggle.com/competitions/hubmap-organ-segmentation/overview/description <br>

## Understanding competition
[Competition description page here](https://www.kaggle.com/competitions/hubmap-organ-segmentation/overview/description) <br>

### description
5つのヒト臓器の機能的組織単位（FTU: functional tissue units）を特定・segmentationすることが目的．<br>
FTU：臓器の主な生理的機能を果たす細胞集団．<br>
The purpose is to identify and segment the functional tissue units (FTUs) of five human organs. <br>
FTUs: Cell populations that perform the major physiological functions of organs. <br>

### evaluation
Dice coefficientによる．定義式が与えられているのでそれに従って計算すればよい．<br>
微分可能かはさておき，目的関数としてそのまま使うことはできそう．<br>
It is based on the Dice coefficient. The definition is given and can be calculated accordingly. <br>
Whether it is differentiable or not, it can be used as the objective function. <br>
- 提出ファイルフォーマットが特殊で，run-length符号化を行う．<br>
Run-length encoding is used. <br>
- segmentation領域を表すマスクはbinary値（0 or 1）で与える．ある画像に映った全てのオブジェクトはbinaryの単一マスクでsegmentationする？（多分）<br>
The mask representing the segmentation region is given as a binary value (0 or 1). Are all objects in an image segmented by a single binary mask? （Maybe.) <br>

### others
- "Freely & publicly available external data is allowed, including pre-trained models"とあるため，例えばImageNetで事前学習したモデルをチューニングしたりできる．<br>
- [Judges Prize](https://www.kaggle.com/competitions/hubmap-organ-segmentation/overview/judges-prize)なるものがあり，自身の解法についてプレゼンする機会があるらしい．挑戦してみたい．<br>

## Understanding dataset
留意点として，データセットが2つの異なるコンソーシアム（HPA, HuBMAP）から提供されている．<br>
It is important to note that the datasets are provided by two different consortia (HPA, HuBMAP).<br>
<b>訓練データはHPA公開データのみからなる．一方，publicテストデータはHPA非公開データ＋HuBMAPデータの組み合わせから，privateテストデータはHuBMAPデータのみからなる．</b><br>
この「属性の異なる2つのデータセット」は公式からも中心的課題の1つであると提示されている("Adapting models to function properly when presented with data that was prepared using a different protocol will be one of the core challenges of this competition.")．<br>
実際のところ，このことはモデル構築と汎化に際してどこまで問題となるのだろうか？<br>
In practice, to what extent is this a problem in model construction and generalization?<br><br>

- [train/test].csv <br>
テーブルデータ．画像と寸法に関するデータに加え，正解マスクのrun-length符号，age，sex（後ろ3つはtrain.csvのみ）が与えられている．<br>
最初の列に画像idが与えられているため，後述の画像ファイルと符合して学習／推論をすることになるだろう．<br>
- [train/test]_images/ <br>
画像ファイル．<b>HPAとHuBMAPとで画像のピクセル数とサンプル染色方法が異なる．</b>染色については門外漢だが，色の具合が違ったり，着色箇所に「クセ」や偏りがあったりするのだろうか？<br>
画像の前処理を相当上手くやる必要がありそう．<br>
また，全ての画像に少なくとも1つのFTU（segmentation対象）が存在する．「FTUが1つもない」という状況は想定しなくて良いだろう．<br>
- train_annotations <br>
ポリゴンマスク境界のアノテーション...らしい．どうやって使うんだろう？<br>

## What to do?
- EDA．また，[公式のdiscussion](https://www.kaggle.com/competitions/hubmap-organ-segmentation/discussion/332666)を読む．
- 上記と並行して，[前回コンペ](https://www.kaggle.com/competitions/hubmap-kidney-segmentation)のdiscussionやcodeを読み漁る
- Bulid first model (Unet?)
- Understand how to submit prediction


[Robust and generalizable segmentation of human functional tissue units](https://www.biorxiv.org/content/10.1101/2021.11.09.467810v1)<br>
Leah L. Godwin, Yingnan Ju, Naveksha Sood, Yashvardhan Jain, Ellen M. Quardokus, Andreas Bueckle, Teri Longacre, Aaron Horning, Yiing Lin, Edward D. Esplin, John W. Hickey, Michael P. Snyder, N. Heath Patterson, Jeffrey M. Spraggins, Katy Börner<br>
bioRxiv 2021.11.09.467810; doi: https://doi.org/10.1101/2021.11.09.467810<br>
<b>以前のsegmentationコンテストの結果の概要をまとめたpre-print論文．</b>公式のdiscussionで紹介があった．かなり重要なのでは？？<br>
Pre-print paper summarizing the results of a previous segmentation contest given by official discussion. Quite important, isn't it?<br>
