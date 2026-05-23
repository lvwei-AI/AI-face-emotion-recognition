# AI 人脸表情识别系统

[![TensorFlow.js](https://img.shields.io/badge/TensorFlow.js-FF6F00?style=flat&logo=tensorflow&logoColor=white)](https://www.tensorflow.org/js)
[![Face-API.js](https://img.shields.io/badge/Face--API.js-4285F4?style=flat&logo=google&logoColor=white)](https://github.com/vladmandic/face-api)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/zh-CN/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
[![Live Demo](https://img.shields.io/badge/🚀%20在线体验-xiaoqintao.com-brightgreen)](http://xiaoqintao.com)

> 基于深度学习的人脸表情实时识别系统，支持浏览器端本地模型训练与推理

🚀 **[在线体验](http://xiaoqintao.com)** - 无需安装，打开浏览器即可体验

## 项目简介

本项目是一个**纯前端实现的人脸表情识别系统**，利用 TensorFlow.js 和 Face-API.js 在浏览器中完成人脸检测、特征提取和表情分类。无需后端服务器，所有计算均在客户端完成，保护用户隐私。

### 核心能力

- 🎯 **实时检测**: 摄像头视频流实时人脸检测与表情分析
- 🧠 **深度学习**: 基于 CNN 卷积神经网络的自定义模型训练
- 🎭 **8种表情**: 支持中性、开心、悲伤、愤怒、惊讶、恐惧、厌恶、轻蔑
- 🔒 **隐私保护**: 纯前端运行，数据不上传服务器
- 📱 **跨平台**: 支持桌面端和移动端浏览器

## 技术架构

```
┌─────────────────────────────────────────────────────────────┐
│                     应用层 (Application)                     │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │ 实时识别模块  │  │ 模型训练模块  │  │ 数据可视化模块    │  │
│  │  (app.js)    │  │  (train.js)  │  │   (UI/Canvas)    │  │
│  └──────────────┘  └──────────────┘  └──────────────────┘  │
├─────────────────────────────────────────────────────────────┤
│                     推理层 (Inference)                       │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────┐  │
│  │           TensorFlow.js (模型推理引擎)                 │  │
│  │  ┌──────────────┐  ┌──────────────────────────────┐ │  │
│  │  │ 预训练模型    │  │      自定义 CNN 模型         │ │  │
│  │  │ Face-API.js  │  │  (浏览器端训练 & 推理)        │ │  │
│  │  └──────────────┘  └──────────────────────────────┘ │  │
│  └──────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────┤
│                     数据层 (Data Layer)                      │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │ 摄像头视频流  │  │ 图像预处理    │  │ 表情标签数据集    │  │
│  └──────────────┘  └──────────────┘  └──────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## 技术栈

| 技术 | 用途 | 版本 |
|------|------|------|
| **TensorFlow.js** | 深度学习框架，模型训练与推理 | 4.10.0 |
| **Face-API.js** | 人脸检测与特征点识别 | 1.7.12 |
| **HTML5 Canvas** | 图像渲染与可视化 | - |
| **WebRTC** | 摄像头视频流获取 | - |
| **Vanilla JS** | 业务逻辑实现 | ES6+ |

## 功能模块

### 1. 实时表情识别 (index.html + app.js)

基于 Face-API.js 预训练模型的实时表情检测：

- ✅ 人脸检测与对齐
- ✅ 7种基本表情识别（FER2013标准）
- ✅ 置信度评分
- ✅ 实时 FPS 显示
- ✅ 可视化表情标签

```javascript
// 核心检测流程
faceapi.detectAllFaces(video, new faceapi.TinyFaceDetectorOptions())
    .withFaceLandmarks()
    .withFaceExpressions()
```

### 2. 自定义模型训练 (train.html + train.js)

使用 TensorFlow.js 在浏览器中训练 CNN 模型：

- ✅ 数据采集：摄像头实时捕获表情样本
- ✅ 数据增强：自动扩增训练数据
- ✅ 模型架构：自定义 CNN 网络
- ✅ 训练可视化：实时损失曲线
- ✅ 模型导出：下载训练好的模型

```javascript
// CNN 模型架构
model = tf.sequential({
    layers: [
        tf.layers.conv2d({ filters: 32, kernelSize: 3, activation: 'relu' }),
        tf.layers.maxPooling2d({ poolSize: 2 }),
        tf.layers.conv2d({ filters: 64, kernelSize: 3, activation: 'relu' }),
        tf.layers.maxPooling2d({ poolSize: 2 }),
        tf.layers.flatten(),
        tf.layers.dense({ units: 128, activation: 'relu' }),
        tf.layers.dropout({ rate: 0.5 }),
        tf.layers.dense({ units: 8, activation: 'softmax' }) // 8种表情
    ]
});
```

### 3. 自定义模型推理 (predict.js)

加载本地训练的模型进行表情预测：

- ✅ 加载自定义 TensorFlow.js 模型
- ✅ 结合 Face-API.js 进行人脸定位
- ✅ 8种表情分类（含轻蔑）
- ✅ 模型热切换（预训练/自定义）

## 支持的8种表情

| 表情 | 英文 | 图标 | 颜色 |
|------|------|------|------|
| 中性 | neutral | 😐 | #74b9ff |
| 开心 | happy | 😊 | #fdcb6e |
| 悲伤 | sad | 😢 | #74b9ff |
| 愤怒 | angry | 😠 | #ff7675 |
| 惊讶 | surprised | 😲 | #fd79a8 |
| 恐惧 | fearful | 😨 | #a29bfe |
| 厌恶 | disgusted | 🤢 | #00b894 |
| 轻蔑 | contempt | 😏 | #fab1a0 |

## 快速开始

### 方式一：直接打开

```bash
# 克隆仓库
git clone https://github.com/lvwei-AI/ai-face-emotion-recognition.git
cd ai-face-emotion-recognition

# 直接在浏览器中打开 index.html
# 或使用本地服务器
python -m http.server 8000
# 访问 http://localhost:8000
```

### 方式二：模型训练流程

1. 打开 `train.html`
2. 选择要训练的表情类别
3. 点击"自动采集"或手动拍照收集样本
4. 点击"开始训练"训练 CNN 模型
5. 导出模型文件
6. 在 `predict.js` 中加载自定义模型

## 项目结构

```
ai-face-emotion-recognition/
├── index.html          # 实时表情识别主页面
├── app.js              # 实时识别逻辑（Face-API.js）
├── train.html          # 模型训练页面
├── train.js            # 训练逻辑（TensorFlow.js）
├── predict.js          # 自定义模型推理
└── README.md           # 项目说明
```

## 核心算法

### 人脸检测
- **模型**: TinyFaceDetector (轻量级)
- **输入**: 视频流帧 (640x480)
- **输出**: 人脸边界框 + 置信度

### 表情识别（预训练模型）
- **模型**: Face-API.js Expression Model
- **基础**: FER2013 数据集训练
- **输出**: 7种表情概率分布

### 自定义 CNN 模型
```
输入层: 96x96x3 (RGB图像)
  ↓
Conv2D(32, 3x3) + ReLU + MaxPool
  ↓
Conv2D(64, 3x3) + ReLU + MaxPool
  ↓
Flatten
  ↓
Dense(128) + ReLU + Dropout(0.5)
  ↓
Dense(8) + Softmax
  ↓
输出: 8种表情概率
```

## 浏览器兼容性

| 浏览器 | 版本要求 | 支持状态 |
|--------|---------|---------|
| Chrome | 80+ | ✅ 完全支持 |
| Firefox | 75+ | ✅ 完全支持 |
| Safari | 13+ | ✅ 支持 |
| Edge | 80+ | ✅ 完全支持 |

**注意**: 需要摄像头权限，建议在 HTTPS 或 localhost 环境下使用。

## 性能指标

| 指标 | 数值 | 说明 |
|------|------|------|
| 检测帧率 | 15-30 FPS | 取决于设备性能 |
| 模型加载时间 | 3-5s | 首次加载 |
| 推理延迟 | < 100ms | 单帧处理时间 |
| 人脸检测精度 | > 90% | 正脸场景 |

## 应用场景

- 🎮 游戏交互：根据表情调整游戏难度
- 📊 用户研究：分析用户观看内容时的情绪反应
- 🏥 心理健康：辅助情绪监测与评估
- 🎓 在线教育：监测学生听课专注度
- 🔒 人机交互：更自然的智能助手

## 技术亮点

1. **纯前端 AI**: 无需服务器，保护隐私
2. **实时处理**: WebGL 加速，流畅体验
3. **模型可训练**: 浏览器端自定义模型训练
4. **模块化设计**: 检测、训练、推理分离
5. **可视化反馈**: 实时数据展示

## 未来优化方向

- [ ] 引入更轻量级的 MobileNet 模型
- [ ] 添加表情时序分析（情绪变化趋势）
- [ ] 多人脸同时检测与表情识别
- [ ] 模型量化压缩，提升移动端性能
- [ ] 添加年龄、性别识别扩展

## 参考资源

- [TensorFlow.js 官方文档](https://www.tensorflow.org/js)
- [Face-API.js GitHub](https://github.com/vladmandic/face-api)
- [FER2013 数据集](https://www.kaggle.com/datasets/msambare/fer2013)

## 作者

**算云软件开发工作室**

专注于人工智能应用开发，提供前沿的技术解决方案。

---

*本项目展示浏览器端深度学习应用能力*
