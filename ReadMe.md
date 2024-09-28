# ex_machina_hardware
![png](https://github.com/user-attachments/assets/f5d5cd54-e71c-4172-bd99-3eae940944e9 "ex-machina")

近藤科学社製KXR-L2を自律化し，株式会社アールティが主催する大会 Humanoid Autonomous Challenge (HAC)に参加するためのサンプルプログラムです．
Hierarchical Task Network（階層化型タスクネットワーク）に基づいて，周囲の環境を認識しながらロボットが次の行動を計画，実行します．<br>

![png](https://github.com/rt-net/KXR_HAC_Software/assets/103564180/9fd10c19-fb3d-4425-acf8-540438e32e46 "HTN Planner")

Humanoid Autonomous Challengeについては[こちらの記事](https://www.rt-shop.jp/blog/archives/10714)を参照してください．<br>

KXR-L2にRaspberryPi Zero 2W, WEBカメラ，IMUなどを増設した[KXR_HAC_Hardware](https://github.com/rt-net/KXR_HAC_Hardware.git)上で動作することを想定しています．他のロボットハードウェアで使用する際は，動作中に呼び出すモーションやパラメータを適宜編集してください．

また，[こちらの連載](https://rt-net.jp/humanoid/archives/category/developer/tsuno-two-legged)で詳細な解説を行っていますので，合わせてご参照ください．<br>

## 動作環境
以下の環境にて動作確認を行っています．
- Python 3.9.2
- Heart to Heart 4 (v2.4.0)
- Linux OS
    - Raspbian GNU/Linux 11 (bullseye)
- コンソールPC
    - Windows 10 Home
    - VNC Viewer 6.22.515
- ロボット
    - KXR-L2（KXR_HAC_Hardware準拠）

環境構築については，[こちらの記事](https://rt-net.jp/humanoid/archives/5037)も参照してください．

## フォルダ構成
<pre>
├── README.md
├── parameterfile.py
├── HTN_planner.py
├── run_HTN_planner.py
├── vision
│   └── vision_library.py
├── motion_control
│   ├── Rcb4BaseLib.py
│   └── motion_control_library.py
├── task_execute
│   └── task_execute_library.py
├── tmp
│   ├── dist.csv
│   ├── left_corner_template.jpg
│   ├── left_corner_template_gray.jpg
│   ├── left_corner_template_wide_gray.jpg
│   ├── mtx.csv
│   ├── right_corner_template.jpg
│   ├── right_corner_template_gray.jpg
│   └── right_corner_template_wide_gray.jpg
├── HTN_sample
│   ├── planning_library_sample.py
│   └── run_HTN_planner_sample.py
├── sample
│   ├── detect_goal_sample.py
│   ├── htn_test.py
│   ├── motion_control_library_sample.py
│   ├── motion_planning_test.py
│   └── vision_library_test.py
└── HeartToHeart_project
    ├── HAC_KXR_HTH4_project.csv
    ├── HAC_KXR_WALK_FORWARD.xml
    ├── HAC_KXR_WALK_LEFT.xml
    ├── HAC_KXR_WALK_RIGHT.xml
    ├── HAC_KXR_TURN_LEFT.xml
    ├── HAC_KXR_TURN_RIGHT.xml
    ├── HAC_KXR_TOUCH_BALL.xml
    ├── HAC_KXR_STAND_UP.xml
    └── その他，KXR-L2G_Battleのサンプルモーション

 </pre>

## ファイル説明
```parameterfile.py```<br>
画像認識，行動計画等のパラメータを一元的に格納したファイルです．<br>

```HTN_planner.py```<br>
Hierarchical Task Networkに基づき動作を選択するためのモジュールです．<br>

```run_HTN_planner.py```<br>
HTN_plannerを用いて，実際にロボットにHAC競技を行わせます．

### vision
```vision_library```<br>
カメラを用いた画像認識関連のクラス，関数をまとめたファイルです．
### motion_control
```motion_control_library```<br>
ロボットに歩行や起き上がりなどのモーション再生の指令を送るクラス，関数をまとめたファイルです．各モーションデータはロボット側のコントロールボード RCB-4HVが保持しています．
```Rcb4BaseLib.py```<br>
近藤科学社の提供する，Pythonが動作する環境からコントロールボード RCB-4HVと連携するためのライブラリです．ロボットに動作指令を送る際は，このライブラリから関数を呼び出して用います．<br>
### task_execute
```task_execute_library```<br>
画像認識結果に基づき移動などの行動を行うクラス，関数をまとめたファイルです．各行動の関数が，HTN PlannerにおけるPrimitive Taskとして扱われます．
### tmp
画像認識関連のテンプレート，パラメータを格納したフォルダです．<br>

```left_corner_template.jpg``` ```right_corner_template.jpg```<br>
テンプレートマッチングに用いる左右のコーナーのテンプレートのカラー版です．<br>

```left_corner_template_gray.jpg``` ```right_corner_template_gray.jpg```<br>
テンプレートマッチングに用いる左右のコーナーのテンプレートのグレースケール版です．<br>

```left_corner_template_wide_gray.jpg``` ```right_corner_template_wide_gray.jpg``` ```mtx.csv``` ```dist.csv```<br>
テンプレートマッチングに用いる左右のコーナーのテンプレートのグレースケール版です．カメラ画角の端にあっても検出できるように，変形が施されています．<br>
### HTN_sample
ロボットを動かさず，HTN_plannerの動作だけ確認する際のサンプルコードが格納されているフォルダです．<br>

```planning_library_sample```<br>
ロボットを動かす際の```motion_planning_library```に相当します．ロボットを動かさず，行動の流れをテキストで示します．<br>

```run_HTN_planner_sample```<br>
ロボットを動かさずにHTN_plannerを実行し，行動計画の流れを確認できます．<br>
### sample
画像認識やモーション再生などの動作を確認するためのサンプルファイルが格納されているフォルダです．

### HeartToHeart_project
近藤科学製のモーション作成ソフト「HeartToHeart4」用のプロジェクトファイルです．
KXR_HAC_Hardwareに合わせてモーションを編集しています．
使用する際は，KXRに搭載されたRCB-4 miniにこのプロジェクトを書き込んでください．

## 実行方法
1. ロボット，RaspberryPiを起動し．フィールドにロボットを配置する．
2. RaspberryPiと同じネットワークにあるコンソールPCで，VNC Viewerを立ち上げRaspberryPiのデスクトップにアクセスする．
3. RaspberryPiでターミナルを起動し，```run_HTN_planner```を実行する．

## 免責事項
当ソフトウェアの使用中に生じたいかなる損害も株式会社アールティでは一切の責任を負いかねます。 

## ライセンス
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />この 作品 は <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">クリエイティブ・コモンズ 表示 4.0 国際 ライセンス</a>の下に提供されています。

```Rcb4BaseLib.py```のみ[MIT License](http://opensource.org/licenses/mit-license.php)

## 作者
TaikiTsuno

## 参考
近藤科学　KXR アドバンスセットA  商品ページ<br>
(https://kondo-robot.com/product/03158)<br>
アールティ　ヒューマノイドロボットブログ<br>
(https://rt-net.jp/humanoid/)
