## MonoUNI: 统一的车辆和基础设施端单目3D目标检测网络（具有充足的深度线索）

:fire::fire:**[NeurIPS 2023]** 论文"[MonoUNI: A Unified Vehicle and Infrastructure-side Monocular 3D Object Detection Network with Sufficient Depth Clues](https://openreview.net/pdf?id=v2oGdhbKxi)"的官方实现

:fire::fire:| [论文](https://openreview.net/pdf?id=v2oGdhbKxi) | [MonoUNI微信解读](https://mp.weixin.qq.com/s/NpLjZT2yuiV-dhIyTcdYRw)

 <div align=center> <img title='MonoUNI' src="imgs/MonoUNI_Poster.png"> </div>

## 项目介绍

本项目基于 MonoUNI 开源项目进行昇腾生态适配，用于参加 2025 昇腾 AI 创新大赛。MonoUNI 是一种统一的车辆和基础设施端单目 3D 目标检测网络，通过充分的深度线索实现高精度检测。

在原始项目基础上，我们进行了昇腾 NPU 适配工作，使项目能够在华为昇腾 AI 处理器上运行，支持使用 NPU 进行模型训练和推理。适配内容包括：
- 集成 Ascend CANN 工具包
- 修改 PyTorch 代码以适配 torch-npu
- 优化数据加载和处理流程以适应 NPU 加速

## 参赛目的与项目愿景

本项目旨在利用昇腾边缘端 NPU 设备，将先进的 3D 目标检测技术应用于道路安全领域。具体来说，我们计划将训练好的模型部署在道路边的路端摄像头上，通过实时分析道路场景中的车辆、行人、骑行者等目标的 3D 位置和运动状态，识别并预测可能发生的碰撞等危险事故。

通过在边缘端部署模型，我们可以实现低延迟的实时危险检测，为智能交通系统提供关键的安全预警信息。这将有助于：
- 提前预警潜在的道路交通事故
- 为交通管理部门提供实时决策支持
- 降低道路交通事故发生率，提升道路安全水平

我们相信，将先进的 AI 技术与昇腾硬件平台相结合，能够为智能交通和自动驾驶领域带来重要贡献。

## 技术方案

本项目基于 MonoUNI 网络架构，利用 3D 立方体深度信息增强空间感知能力。通过考虑俯仰角和焦距的多样性，提出了归一化深度这一统一优化目标，实现了车辆端和路端检测问题的统一。同时，开发了障碍物的 3D 归一化立方体深度来促进深度信息的学习。

我们将该技术方案移植到昇腾平台，充分发挥 NPU 在 AI 推理方面的性能优势，实现高效的边缘端实时 3D 目标检测。

## 新闻动态
- [x] ***[20231016]*** 创建仓库
- [x] ***[20240314]*** 发布 Rope3D 数据集，合并为4个类别并进行ROI过滤以供评估
- [x] ***[20240315]*** 发布 MonoUNI 提出的3D立方体深度数据（包含在Rope3D数据集中）
- [x] ***[20240326]*** 发布初始训练/验证代码
- [x] ***[20240326]*** 发布 Rope3D 上的 MonoUNI 检查点
- [x] ***[20240326]*** 支持 Rope3D 数据集
- [ ] 支持 DAIR-V2X-I 数据集
- [ ] 支持 KITTI 数据集

## 安装说明
a. 克隆此仓库。
~~~
git clone https://github.com/Traffic-X/MonoUNI
~~~

b. 安装依赖库如下：
* 使用conda创建新环境
~~~
conda create -n rope3d python=3.8
~~~

* 激活环境
~~~
conda activate rope3d
~~~

* 安装依赖的Python库：
~~~
pip3 install torch==2.1.0 pyyaml setuptools torch-npu==2.1.0.post13
# 指定官方源安装
pip install torchvision==0.16.0 --index-url https://download.pytorch.org/whl/cpu
# 下载Torchvision Adapter代码，进入插件根目录
git clone https://gitcode.com/ascend/vision.git vision_npu
cd vision_npu
git checkout v0.16.0-6.0.0
# 安装依赖库
pip3 install -r requirement.txt
# 初始化CANN环境变量
source /usr/local/Ascend/ascend-toolkit/set_env.sh # 默认路径，如有需要请更改。
# 编包
python setup.py bdist_wheel
# 安装
cd dist
pip install torchvision_npu-0.16.*.whl
# 默认路径，如有需要请更改。
#运行以下命令初始化CANN环境变量
source /usr/local/Ascend/ascend-toolkit/set_env.sh
~~~

## 数据集
- [x] 从[**这里**](https://pan.baidu.com/s/1Tt014qMNcDxAMCkEWH_EZQ?pwd=d1yd)下载官方 Rope3D 数据集。
    ~~~
    tar -zxvf Rope3D_data.tar.gz
    ~~~
    目录结构如下：
    Rope3D_data
    ├── box3d_depth_dense
    ├── calib
    ├── denorm
    ├── extrinsics
    ├── image_2
    ├── ImageSets
    ├── label_2
    ├── label_2_4cls_filter_with_roi_for_eval
    └── label_2_4cls_for_train

- [ ] 支持 DAIR-V2X-I 数据集
- [ ] 支持 KITTI 数据集

## 训练
- [x] Rope3D 数据集

    修改 config.yaml 中的 'root_dir'，使用您自己的 'Rope3D_data' 下载路径
    ~~~
    bash train.sh
    ~~~
- [ ] DAIR-V2X-I 数据集
- [ ] KITTI 数据集

## 评估
- [x] Rope3D 数据集

    修改 config.yaml 中的 'root_dir'，使用您自己的 'Rope3D_data' 下载路径
    修改 config.yaml (tester) 中的 'resume_model'，使用您自己的检查点路径
    ~~~
    bash eval.sh
    ~~~
- [ ] DAIR-V2X-I 数据集
- [ ] KITTI 数据集

## 权重
从[**此处**](https://pan.baidu.com/s/13H8CJzwuDISGR4q6MRg3sg?pwd=g86j)下载检查点 (Rope3D)

## 引用
如果您觉得 MonoUNI 对您的研究有帮助，请考虑给个星标 ⭐ 并引用：
~~~
@inproceedings{jia2023monouni,
title={MonoUNI: A Unified Vehicle and Infrastructure-side Monocular 3D Object Detection Network with Sufficient Depth Clues},
author={Jinrang Jia and Zhenjia Li and Yifeng Shi},
booktitle={Thirty-seventh Conference on Neural Information Processing Systems},
year={2023},
url={https://openreview.net/forum?id=v2oGdhbKxi}
}
~~~
## 致谢
非常感谢以下代码对构建本代码库的大力帮助：
- [GUPNet](https://github.com/SuperMHP/GUPNet/tree/main)
- [DID-M3D](https://github.com/SPengLiang/DID-M3D)
- [MonoLSS](https://github.com/Traffic-X/MonoLSS)
- [BEVHeight](https://github.com/ADLab-AutoDrive/BEVHeight)