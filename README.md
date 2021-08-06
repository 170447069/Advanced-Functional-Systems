# 機能システム学特論課題 
## MoveIt!を使ったマニピュレータの制御
### ROS/MoveIt!によるマニピュレータの制御 
ターミナル[1]で実行
```
roslaunch sixdofarm sixdofarm.launch
```
 作成した6自由度アームを操作するGUIが表示される
 <br>
 ターミナル[2]で実行
```
rviz
```
rvizを起動したら「Displays」の「FixedFrame」を「base」に設定
<br>
「Add」を押し,「By display type」の中にある「RobotModel」を押す
```
roslaunch sixdofarm_moveit_config demo.launch
```
rviz上にロボットマニュピレータが表示される
<br>
### Gazeboによるマニピュレータのシミュレーション
```
roslaunch gazebo_ros empty_world.launch
```
gazeboが立ち上がる
<br>
ターミナル[1]で実行
```
roslaunch sixdofarm_gazebo sixdofarm.launch
```
gazebo画面が立ち上がり,ガソリンスタンドの中に色付きロボットが倒立している
<br>
ターミナル[2]で実行
```
roslaunch sixdofarm_control sixdofarm_control.launch
```
pidという項目で，PIDのゲインを設定してある
<br>
ターミナル[3]で実行
```
rqt
```
プロットしたいトピックを記載する
<br>
ex)/sixdofarm/joint1_position_controller/command/data　
<br>
Message Publisherのtopickのチェックボックスにチェックを入れる
<br>
```
roslaunch sixdofarm_moveit_config sixdofarm_moveit.launch
```
gazeboとrvizが一つのコマンドで実行できる
<br>
## ROSを使った移動ロボットの制御
### Gazebo上での移動ロボットモデルの構築
```
roslaunch wheel_robot wheel_robot.launch
```
gazebo画面が立ち上がり4輪の移動ロボットが表示
<br>
ターミナル[1]で実行
```
roslaunch wheel_robot wheel_robot_control.launch
```
gazeboとrvizが立ち上がる
<br>
ターミナル[2]で実行
```
rqt
```
プロットしたいトピックを記載する
<br>
ターミナル[3]で実行
```
cd catkin_ws/src/wheel_robot_control/scripts/
```
```
python key_teleop.py
```
モデルをキーボードで操作できる
### 移動ロボットのナビゲーション
ターミナル[1]で実行
```
roslaunch wheel_robot wheel_robot.launch
```
gazeboが起動される
<br>
ターミナル[2]で実行
```
rosrun odom_listener odom_listener
```
odometryのトピックを受信する
<br>
ターミナル[3]で実行
```
cd catkin_ws/src/wheel_robot_control/scripts/
```
```
python key_teleop.py
```
モデルをキーボードで操作できる
<br>
ロボットの走行軌跡をyamlで保存できる
<br>
ターミナル[1]で実行
```
roslaunch wheel_robot wheel_robot_control.launch
```
ターミナル[2]で実行
```
roslaunch wheel_robot_nav wheel_robot_nav.launch
```
上で記録したロボットの走行軌跡を自動で移動する
## 移動ロボットで地図を作成（slam gmapping)
ターミナル[1]で実行
```
roslaunch wheel_robot wheel_robot.launch
```
LRFがついた4輪モデルが表示
<br>
gazbo上に障害物を配置する
<br>
ターミナル[2]で実行
```
roslaunch wheel_robot_gmapping wheel_robot_gmapping.launch
```
「sensor_msgs./LaserScan」トピックとtfのデータを読み込んで（sabscribe)して，地図データ「nav_msgs/OccupancyGrid」を出力
<br>
ターミナル[3]で実行
```
rviz
```
「Add」から「RobotModel」と「Map」を選択して左側のリストに追加
<br>
ターミナル[4]で実行
```
cd catkin_ws/src/wheel_robot_control/scripts/
```
```
python key_teleop.py
```
モデルをキーボードで操作できる
## 差動２輪移動ロボットの作り方
ターミナル[1]で実行
```
roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
建物内に差動2輪移動ロボットが表示される
<br>
ターミナル[2]で実行
```
roslaunch wheel_robot_gmapping wheel_robot_gmapping.launch
```
移動ロボットのナビゲーションを行うために地図作成
<br>
ターミナル[3]で実行
```
rviz
```
「Add」から「RobotModel」と「Map」を選択して左側のリストに追加
<br>
ターミナル[4]で実行
```
cd catkin_ws/src/wheel_robot_control/scripts/
```
```
python key_teleop.py
```
ターミナル[5]で実行
```
rosrun map_server map_saver -f testmap
```
testmapという名前で作成した地図を保存
# 課題
## Willow Garageの建物内の経路を２．１のナビゲーションで習ったWaypoitを記録していく方法で作成し，目標位置までナビゲーションをおこなプログラムを作成せよ．
```
roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
```
roslaunch diff_mobile_robot amcl.launch
```
```
rviz
```
```
roslaunch diff_mobile_robot diff_mobile_nav.launch
```
