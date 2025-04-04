### Fork from https://github.com/super95shao/shao-s-Flight-Control-with-cc-vs

### 此通用飞控是在上一代四旋翼螺旋桨飞控的基础上重写而来。
### 上一代的四旋翼飞控，原理是使用cc控制转速控制器的转速，修改物理轴承螺旋桨（襟翼轴承+flap）带来的升力、反扭力，进而实现姿态闭环控制。
### 但也因此，部署、调参 都非常麻烦，不便于复制到其它机型上，通用性差。

### 所以这版通用飞控便诞生了，它使用cc:vs的扩展API作为动力源，可以实现开箱即用，一键部署，十分的方便。xxx
### 新版本不再依赖ccvs，而是将所有动力改为虚空动力(void power)mod，
### 我的目标是使它可以模拟出各种类型的飞行器的操作手感，并具备自动驾驶、寻路、避障等基本功能

### 完整功能需要安装Kallen的MetaPhysics的扫描器API（follow模式、hms_fly模式、切换玩家名等功能需要）
### https://github.com/KallenKas024/Metaphysics/

> 特别感谢：
@kallen 提供的metaphysics mod ： 飞控的雷达扫描器、射线检测（方块扫描器）使用了此mod提供的API
感谢 @kallen 因我的需求增加mod功能

## newVersion是新版本，重构了核心部分，依赖虚空动力mod，并支持虚空动力的全息屏
> 增加了全息显示器GUI (void power) , 动力从ccvs改为虚空动力的虚空引擎
> 部署需要一个引擎控制器外设（只能放1个）， 还需要虚空引擎（任意数量，不需要作为外设，只要放在船上就行）
### https://github.com/dfdyz/VoidPowerMod

> 
### 更新日志：
* 2024/4/05 增加多旋翼模式（quadRotor）：姿态模式、定点模式
* 2024/4/10 增加跟随模式（follow）：跟随玩家 --玩家扫描器API来自@kallen的玄学mod
* 2024/4/20 增加定点循环模式（pointLoop）：在各目标点之间循环飞行（无避障）
* 2024/5/25 增加星舰模式（spaceShip）：手动模式（角速度闭环+速度闭环）、定点模式（角度闭环+位置闭环）
* 2024/5/29 增加穿越机模式（quad_FPV）：（角速度闭环）
* 2024/5/31 增加天基炮模式（hms）：从玩家视线发射激光、检测到方块就设为目标，并将飞行器头部对准目标方块 --扫描器API来自@kallen的玄学mod
* 2024/6/01 增加鼠标飞行模式（hms）：同天基炮模式，但增加控制器直驱
* 2024/6/02 增加风阻、重力增强（quad_FPV）：为穿越机模式增加空气阻力模拟、重力模拟（在瓦尔基里本身重力的基础上增强）--建议来自@FEMO114
* 2024/6/04 穿越机模式增加：辅助悬停模式（角度闭环+速度闭环）
* 2024/6/05 增加调参系统，并制作了第一版的GUI界面
* 2024/6/06 增加锁定模式按钮-可以在gui中切换锁定
* 2024/6/08 增加直升机模式
* 2024/6/09 直升机模式增加独立调参界面：可以单独设置直升机模式的各种参数、以便模拟不同类型的直升机手感
* 2024/6/12 增加多显示器支持：增加界面管理器，可以同时连接多个显示器，共同操作飞控 --由 @siiftun1857 提出并实现（贡献代码）by @siiftun1857 
* 2024/6/19 增加物理帧模式：区别于之前的模式，该模式下抓取物理帧作为飞控运行频率。（如果没有抓取到则启用游戏刻模式0.05s）
* 2024/6/21 增加全角度物理化支持：现在面向任意方向物理化的载具都可以通过set_att界面修改初始朝向，获取正确的姿态解算和控制
* 2024/6/23 增加多窗口实例：现在支持任意尺寸的显示器，分配多窗口副本
* 2024/6/24 界面完全重写：将ATT界面作为主要界面，并增加窗口合并功能、快速调参、控制器输入状态显示等，增加色彩主题修改页面
* 2024/6/25 增加加力燃烧模式（spaceShip）：加力燃烧模式使飞船输出的力增加-可在调参页面修改比例 --建议来自 @我不是柠檬精-_-
* 2024/6/26 增加飞艇模式（airShip）：角度闭环(PITCH、ROLL)=0，可以操作偏航、前后、左右、上下等。类似于船舵，但可以左右平移。
* 2024/6/27 飞艇模式增加独立调参界面
* 2024/7/05 增加摄影机模式: 向目标飞船请求连接，同意后开启摄影机模式-可以围绕目标飞船旋转，并跟随他移动
* 2024/7/06 星舰模式调整：悬停的力独立。现在上下喷口的力可用（跟随旋转）
* 2024/7/08 星舰模式更新：解耦模式
* 2024/7/20 穿越机模式增加Rate（打杆曲线、油门曲线） 同BetaFlight。现实中基于BetaFlight飞控的Rate都可以还原
* 2024/11/1 增加虚空动力版本飞控
* 2024/11/7 增加全息屏幕GUI(虚空动力)
* 2024/11/15 增加全息屏火控锁定界面
* 2024/11/16 增加飞控内置火控雷达
* 2024/11/24 增加弹药显示、雷达目标名称、血量显示
* 2024/12/01 增加飞船网络白名单功能
* 2024/12/17 增加自动配平/定速巡航功能 （星舰模式）
* 2024/12/21 增加录像和回放功能
* 2024/12/22 增加断线重连（开机自动连接上一次连接的父级）
