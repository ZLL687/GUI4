# CFST Load–Strain Prediction · V2.1

基于已训练 V2.1 A/B 模型的 Streamlit 荷载–应变曲线预测应用。模型文件直接随 GitHub 仓库提供。

## 上传哪些文件

解压部署包，将以下 **5 个文件**放到同一个 GitHub 仓库根目录，保留文件名。已有仓库可直接替换同名文件。

```text
README.md
streamlitapp.py
V2.1_AB_models.joblib
requirements.txt
packages.txt
```

| 文件 | 用途 |
| --- | --- |
| `streamlitapp.py` | 应用入口、参数界面、预测调用和结果导出 |
| `V2.1_AB_models.joblib` | 训练完成的 A/B 模型及其参数 |
| `requirements.txt` | Streamlit 界面的 Python 依赖 |
| `packages.txt` | 云端 Linux 系统依赖 |
| `README.md` | 使用与部署说明 |

请上传解压后的文件，不要只上传 ZIP。不要改动或重新保存模型二进制文件。

## 部署到 Streamlit Community Cloud

1. 在 GitHub 中上传或替换上述文件，并提交到 `main` 分支。
2. 在 Streamlit 选择 **Create app → Deploy from repo**。
3. 选择自己的仓库，填写：
   - Branch：`main`
   - Main file path：`streamlitapp.py`
4. 使用平台的 Python 3.14 环境即可，然后点击 **Deploy**。

已有应用在仓库文件更新后会自动更新。`requirements.txt` 发生变化时，平台会重新安装依赖。

## Python 版本与首次预测

- 本版本的界面已适配 Python 3.14，云端日志继续显示 `Using Python 3.14...` 是正常现象。
- 为保持已训练模型的计算环境，程序会在第一次预测时自动准备独立的 Python 3.12 模型环境，并安装指定版本的计算库。
- 模型环境需要访问软件包下载服务。首次准备可能需要几分钟；界面会显示等待提示和已用时间。后续预测会复用该环境。
- 应用休眠、重启或更换云端机器后，首次预测可能再次准备环境。
- 此过程只安装运行依赖、读取已有模型，不进行模型训练。

模型文件 SHA-256：

```text
e21c6a491899a9fbf443337a4c7428697409d1e050ffcc24025e7d4a7ce20354
```

## 使用方法

1. 在侧栏选择 **Model scheme A / B**。
2. 选择输入方式：
   - **Geometry and materials**：输入几何尺寸与材料参数。
   - **Model features**：直接输入模型使用的特征。
3. 设置显示的应变起点、终点与点数，点击 **Predict curve**。
4. 查看曲线、预测带、峰值和表格，可下载 SVG、显示区间 CSV 和完整曲线 CSV。

### 参数单位

- 几何尺寸：mm；截面面积：mm²。
- 混凝土与钢材强度：MPa。
- 在 **Model features** 中，`fcu × Ac`、`fy × As` 的单位为 **N**。
- 荷载输出单位为 **kN**。
- 橡胶替代率输入 0–1 之间的比例，例如 0.20 表示 20%。
- 螺旋角使用角度制。
- 单根圆管截面可选 **Circular section**；已有有效截面面积时可选 **Enter effective areas**。

完整曲线使用 0–0.03 应变范围内的 101 个固定点。其他显示区间和点数通过插值获得，不改变完整曲线的峰值。预测带沿用模型包中的参数。

## 常见问题

### 仍然显示蛋糕图或启动等待页

打开 **Manage app** 查看日志。确认仓库中的 `requirements.txt` 已替换为本包版本；本版界面依赖为 Streamlit 和 uv，不会在 Python 3.14 中安装旧版模型计算库。模型计算库在首次预测时安装到独立环境。

如果日志仍出现旧的 `numpy==1.26.4` 等库在 **Python 3.14** 中编译，先核对当前应用连接的仓库、分支、目录及该目录下的依赖文件。仅更新 `streamlitapp.py` 不足以替换旧依赖配置。

### 提示找不到模型文件

确认 `V2.1_AB_models.joblib` 与 `streamlitapp.py` 位于同一目录，名称完全一致。

### 提示模型文件不匹配

重新上传部署包中的原始 `V2.1_AB_models.joblib`。不要将其他模型改名为该文件，也不要自行重新序列化模型。

### 首次预测提示模型环境准备失败

打开 **Manage app** 查看 `[V2.1]` 开头的日志及其后面的错误。检查软件包下载连接、仓库依赖文件是否匹配，再重试预测。

### 修改了输入但曲线没有变化

修改表单参数后，需要再次点击 **Predict curve**。

## 本地运行

在包含上述文件的目录内执行：

```bash
python -m pip install -r requirements.txt
python -m streamlit run streamlitapp.py
```

Linux 系统需要 `packages.txt` 中列出的系统库。首次预测仍会自动准备模型运行环境。

## 平台文档

- [Streamlit 部署说明](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy)
- [Streamlit 依赖文件说明](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/app-dependencies)
- [uv 的 Python 环境管理](https://docs.astral.sh/uv/guides/install-python/)
