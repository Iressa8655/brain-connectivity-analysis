「我建立了一個腦部連通性分析專案來展示我對神經影像工具的理解。

**第 1 步：預處理** — 我下載了真實的 fMRI 掃描數據，進行 smoothing 和對齐到標準腦模板。這是 UCLA 實驗室的標準做法。![[01_raw_vs_smoothed_fmri.png]]

**第 2 步：連通性提取** — 使用 Nilearn，我從 fMRI 時間序列中提取了 116 個腦區之間的 functional connectivity。結果是一個 116×116 的相關矩陣，顯示哪些腦區協作。![[02_connectivity_matrix.png]]

**第 3 步：網絡分析** — 使用 NetworkX，我計算了 degree centrality、betweenness centrality 和 clustering coefficient。這些指標識別了樞紐區域 — 腦活動的協調者。![[03_network_metrics.png]]

**第 4 步：組別比較** — 我比較了健康人群和 ADHD 患者的連通性模式。你可以清楚看到：ADHD 患者在特定網絡中的連通性明顯降低。![[04_group_comparison.png]]

**為什麼這對 UCLA 重要：** 這個流程正是 Dr. Narr 實驗室用於腦部疾病研究的。我的專案展示我理解 fMRI 前處理、connectivity extraction、network analysis 和臨床解釋。

**與我的研究提案的連結：** 在 UCLA，我想用這個方法來驗證中藥化合物。具體來說：某個人參提取物會改變 ADHD 患者的這些網絡連通性嗎？結合量子 Bayesian inference，我可以用更少的患者來回答這個問題。」
# 腦部網絡項目 — 完全清晰指南
# Brain Network Project — Complete Clear Guide

**如果你現在困惑，這份筆記會解答所有疑問。**

---

## 第一部分：你需要知道的核心概念 (The Core Concept)

### 問題1：我們到底在做什麼？
### Question 1: What exactly are we doing?

**簡單答案：**
我們要寫程式碼來分析真實患者的腦部掃描（fMRI），找出哪些腦區最重要。

**Simple Answer:**
We're writing code to analyze real patient brain scans (fMRI) to find which brain regions are most important.

---

### 問題2：為什麼要做這個？
### Question 2: Why do this?

| 你想達成的目標 | 這個項目怎麼幫助你 |
|---|---|
| **申請UCLA** | 展示你會用 Nilearn + NetworkX 這些工具 |
| **進UCLA後做研究** | 學會分析患者腦部數據的基本技能 |
| **驗證中藥化合物** | 知道哪些腦區受到中藥影響 |
| **使用量子計算** | 知道什麼數據需要被加速處理 |

**Translation:**

| Your Goal | How This Project Helps |
|---|---|
| **Apply to UCLA** | Show you can use Nilearn + NetworkX tools |
| **Research at UCLA** | Learn basic skills to analyze patient brain data |
| **Validate herbal medicines** | Know which brain regions are affected by compounds |
| **Use quantum computing** | Know what data needs quantum speedup |

---

## 第二部分：這個項目會教你什麼？
## Part 2: What Will You Learn?

### 你會學到4個工具：

| 工具名稱 | 用途 | 你會學到什麼 |
|---|---|---|
| **Nilearn** | 處理腦部影像數據 | 如何從 fMRI 提取腦區活動 |
| **NetworkX** | 分析網絡結構 | 如何找出哪些腦區最重要（樞紐 hub） |
| **Matplotlib** | 製作圖表 | 如何視覺化數據結果 |
| **Pandas** | 整理數據 | 如何保存和比較分析結果 |

**英文版本:**

| Tool Name | Purpose | You Will Learn |
|---|---|---|
| **Nilearn** | Process brain imaging data | How to extract brain region activity from fMRI |
| **NetworkX** | Analyze network structure | How to find which brain regions are important (hubs) |
| **Matplotlib** | Create charts | How to visualize data results |
| **Pandas** | Organize data | How to save and compare analysis results |

---

### 你會學到的技能順序：

```
第1天 (2小時)
  ↓
[學習工具] 安裝 Nilearn, NetworkX, Matplotlib
  ↓
[練習技能] 打開真實患者的腦部掃描 (fMRI file)
  ↓
第2天 (1.5小時)
  ↓
[處理數據] 提取116個腦區的活動信息
  ↓
[分析] 計算哪些腦區彼此連接
  ↓
第3天 (1小時)
  ↓
[找出重要區域] 用 NetworkX 找出樞紐區域（hub regions）
  ↓
[比較患者] 對比健康人 vs 生病患者
  ↓
[完成] 推送到 GitHub，準備面試
```

---

## 第三部分：具體步驟（一步一步）
## Part 3: Step-by-Step Process

### 步驟1：下載數據（15分鐘）
### Step 1: Download Data (15 minutes)

**你在做什麼？**
下載公開的患者腦部掃描數據（ADHD200 dataset）。

**What are you doing?**
Downloading public patient brain scan data (ADHD200 dataset).

**代碼：**
```python
from nilearn import datasets

# 下載真實患者數據
data = datasets.fetch_adhd200()

print(f"患者數量: {len(data.func)}")  # How many patients
print(f"第一個患者的檔案: {data.func[0]}")  # File path
```

**你學到什麼？**
- 如何用 Nilearn 下載神經影像數據
- fMRI 檔案長什麼樣子
- 患者數據的結構

---

### 步驟2：打開和清潔數據（30分鐘）
### Step 2: Open and Clean Data (30 minutes)

**你在做什麼？**
打開第一個患者的腦部掃描，去除雜訊。

**What are you doing?**
Opening a patient's brain scan and removing noise.

**代碼：**
```python
from nilearn.image import load_img, smooth_img
import matplotlib.pyplot as plt

# 1. 打開腦部掃描檔案
fmri = load_img(data.func[0])
print(f"腦掃描的形狀: {fmri.shape}")
# 例如: (64, 64, 39, 150) = 64x64x39 像素, 150個時間點

# 2. 清潔數據（去除雜訊）
fmri_clean = smooth_img(fmri, fwhm=6)

# 3. 顯示結果
plt.figure()
plt.imshow(fmri[:, :, 20, 0], cmap='gray')  # 顯示一張圖片
plt.title('真實患者的腦部掃描')
plt.savefig('step1_brain_image.png')
plt.show()
```

**你學到什麼？**
- 怎樣用 Nilearn 打開腦部影像檔案
- fMRI 是 4D 的（3D 空間 + 時間）
- 什麼是 smoothing（平滑化）— 為什麼需要它
- 如何視覺化腦部圖像

---

### 步驟3：提取腦區活動（30分鐘）
### Step 3: Extract Brain Region Activity (30 minutes)

**你在做什麼？**
告訴電腦：「腦部有116個區域，請告訴我每個區域在做什麼。」

**What are you doing?**
Telling the computer: "There are 116 brain regions, tell me what each one is doing."

**代碼：**
```python
from nilearn.input_data import NiftiLabelsMasker
from nilearn.connectome import ConnectivityMeasure

# 1. 加載腦圖譜（116個區域的地圖）
atlas = datasets.fetch_atlas_aal()
print(f"腦區數量: {atlas.labels}")  # Should be 116

# 2. 提取每個區域的活動
masker = NiftiLabelsMasker(labels_img=atlas.maps, standardize=True)
region_activity = masker.fit_transform(fmri_clean)
print(f"區域活動形狀: {region_activity.shape}")
# 例如: (150, 116) = 150個時間點, 116個腦區

# 3. 計算區域之間的相關性
connectivity = ConnectivityMeasure(kind='correlation')
conn_matrix = connectivity.fit_transform([region_activity])[0]
print(f"連通性矩陣形狀: {conn_matrix.shape}")  
# (116, 116) = 116個區域兩兩之間的相關性

# 4. 視覺化
import seaborn as sns
plt.figure(figsize=(10, 8))
sns.heatmap(conn_matrix, cmap='coolwarm')
plt.title('腦區連通性（哪些區域一起工作）')
plt.savefig('step2_connectivity.png')
plt.show()
```

**你學到什麼？**
- 什麼是 brain atlas（腦圖譜）
- 怎樣提取每個腦區的時間序列數據
- 什麼是 connectivity matrix（連通性矩陣） — 它顯示哪些腦區相關
- 如何用熱圖 (heatmap) 視覺化

---

### 步驟4：找出重要的腦區（30分鐘）
### Step 4: Find Important Brain Regions (30 minutes)

**你在做什麼？**
用 NetworkX 計算：哪些腦區是「樞紐」（hub）？哪些區域最連接其他區域？

**What are you doing?**
Using NetworkX to calculate: Which brain regions are "hubs"? Which regions connect to the most others?

**代碼：**
```python
import networkx as nx
import pandas as pd

# 1. 把連通性矩陣轉成網絡圖
G = nx.Graph()

# 加入所有116個腦區
for i in range(116):
    G.add_node(i)

# 加入連接（只保留強的相關性）
threshold = 0.5
for i in range(116):
    for j in range(i+1, 116):
        if conn_matrix[i, j] > threshold:
            G.add_edge(i, j)

print(f"網絡信息: {G.number_of_nodes()} 個腦區, {G.number_of_edges()} 個連接")

# 2. 計算樞紐度（centrality）— 哪些腦區最重要？
centrality = nx.degree_centrality(G)

# 找出前5個最重要的腦區
top_5 = sorted(centrality.items(), key=lambda x: x[1], reverse=True)[:5]
print("\n最重要的5個腦區:")
for region_id, importance in top_5:
    print(f"  腦區 {region_id}: 重要性 = {importance:.3f}")

# 3. 其他重要指標
betweenness = nx.betweenness_centrality(G)  # 哪些區域是「橋樑」？
clustering = nx.clustering(G)  # 哪些區域的鄰居相互連接？

# 4. 保存結果
metrics_df = pd.DataFrame({
    '腦區': range(116),
    '樞紐度': [centrality[i] for i in range(116)],
    '中介度': [betweenness[i] for i in range(116)],
    '聚集度': [clustering[i] for i in range(116)]
})

metrics_df.to_csv('brain_metrics.csv', index=False)
print("\n結果已保存到 brain_metrics.csv")

# 5. 視覺化分佈
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 3, figsize=(15, 4))

axes[0].hist(list(centrality.values()), bins=20, color='blue')
axes[0].set_title('樞紐度分佈')

axes[1].hist(list(betweenness.values()), bins=20, color='green')
axes[1].set_title('中介度分佈')

axes[2].hist(list(clustering.values()), bins=20, color='red')
axes[2].set_title('聚集度分佈')

plt.tight_layout()
plt.savefig('step3_network_metrics.png')
plt.show()
```

**你學到什麼？**
- 什麼是 degree centrality（樞紐度）— 一個區域有多少連接
- 什麼是 betweenness centrality（中介度）— 一個區域有多重要
- 什麼是 clustering coefficient（聚集度）— 一個區域的鄰居有多連接
- 為什麼這些指標對醫療很重要（患病患者的樞紐區域常常受損）

---

### 步驟5：比較患者和健康人（30分鐘）
### Step 5: Compare Patients vs Healthy (30 minutes)

**你在做什麼？**
分析：健康人的腦部連通性 vs ADHD患者的連通性，看看有什麼不同。

**What are you doing?**
Analyzing: Healthy brain connectivity vs ADHD patient connectivity, see the differences.

**代碼：**
```python
import numpy as np

# 1. 分開健康人和患者
healthy_connections = []
adhd_connections = []

for i in range(len(data.func)):
    # 對每個患者做之前的步驟（步驟2-3）
    fmri = load_img(data.func[i])
    fmri_clean = smooth_img(fmri, fwhm=6)
    activity = masker.fit_transform(fmri_clean)
    conn = connectivity.fit_transform([activity])[0]
    
    # 根據診斷分類
    diagnosis = data.phenotype[i]['DX']
    if diagnosis == 1:  # 健康人
        healthy_connections.append(conn)
    else:  # ADHD患者
        adhd_connections.append(conn)

# 2. 計算平均連通性
healthy_avg = np.mean(healthy_connections, axis=0)
adhd_avg = np.mean(adhd_connections, axis=0)
difference = healthy_avg - adhd_avg

print(f"健康人數量: {len(healthy_connections)}")
print(f"ADHD患者數量: {len(adhd_connections)}")

# 3. 視覺化比較
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# 健康人
im1 = axes[0].imshow(healthy_avg, cmap='coolwarm', vmin=-1, vmax=1)
axes[0].set_title('健康對照組的腦連通性')
plt.colorbar(im1, ax=axes[0])

# ADHD患者
im2 = axes[1].imshow(adhd_avg, cmap='coolwarm', vmin=-1, vmax=1)
axes[1].set_title('ADHD患者的腦連通性')
plt.colorbar(im2, ax=axes[1])

# 差異
im3 = axes[2].imshow(difference, cmap='RdBu_r', vmin=-0.5, vmax=0.5)
axes[2].set_title('差異（健康 - ADHD）')
plt.colorbar(im3, ax=axes[2])

plt.tight_layout()
plt.savefig('step4_group_comparison.png')
plt.show()

# 4. 統計分析
print("\n連通性差異統計:")
print(f"最大差異: {np.max(np.abs(difference)):.3f}")
print(f"平均絕對差異: {np.mean(np.abs(difference)):.3f}")
```

**你學到什麼？**
- 如何比較兩組患者
- 如何找出疾病的生物標誌物（biomarker）— 腦連通性差異
- 為什麼某些腦區在患者中連通性更低
- 這怎麼連接到診斷：連通性差異 = 可診斷的信號

---

## 第四部分：這個項目怎麼連接到UCLA研究？
## Part 4: How This Connects to Your UCLA Research?

### 地圖：從現在到UCLA研究

```
現在：腦部網絡項目
├─ 你學會: 提取腦連通性, 找樞紐區域, 比較患者
├─ 工具: Nilearn, NetworkX
└─ 目標: 展示給UCLA你會這些基本技能

     ↓
     ↓ （在UCLA的第1-3週）

UCLA Week 1-3: 腦部疾病研究
├─ 你做: 分析阿茲海默症患者的腦連通性
├─ Dr Narr的實驗室: 教你他們如何用這些數據診斷疾病
└─ 你學到: 哪些腦區在阿茲海默症中最受損

     ↓
     ↓ （在UCLA的第4-8週）

UCLA Week 4-8: 量子計算加速
├─ 新問題: 「可以用量子MCMC快速檢測中藥化合物對腦網絡的影響嗎？」
├─ 工具: Qiskit (量子計算) + PyMC (貝氏推斷)
└─ 目標: 用量子計算驗證中藥，患者數從500人 → 50人

     ↓
     ↓ （你的原創研究）

你的研究整合：
├─ 步驟1（Week 1-3知識）: 中藥化合物影響哪些腦網絡？
├─ 步驟2（Week 4-8知識）: 用量子MCMC檢測那個影響（用更少患者）
├─ 步驟3（寫論文）: 「量子加速中藥驗證：用腦網絡生物標誌物」
└─ 新穎性: 沒有人這樣做過！
```

---

### 具體連接：每個步驟在UCLA會怎樣變化

| 你現在做的 | UCLA會怎樣改進 | 目的 |
|---|---|---|
| 分析ADHD患者 | → 分析阿茲海默症患者 | 換個疾病，同樣技能 |
| 找healthy vs sick差異 | → 找中藥是否改變了那個差異 | 測試中藥是否有效 |
| 用Python和統計學 | → 加上量子計算加速 | 用更少患者證明效果 |
| 用公開數據 | → 用UCLA醫院的真實患者 | 更有意義的研究 |

---

## 第五部分：你應該如何學習？
## Part 5: How Should You Learn This?

### 時間表：2.5小時完成

| 時間 | 任務 | 輸出 |
|---|---|---|
| **15分** | 下載數據 + 安裝工具 | 可以執行 Python 代碼 |
| **30分** | 打開和清潔腦掃描 | 1張圖：`step1_brain_image.png` |
| **30分** | 提取腦區連通性 | 1張圖：`step2_connectivity.png` |
| **30分** | 計算樞紐度和其他指標 | 3張圖：`step3_network_metrics.png` |
| **30分** | 比較患者和健康人 | 1張圖：`step4_group_comparison.png` |
| **15分** | 推送到GitHub | 項目準備好了，可以在面試中展示 |

---

## 第六部分：為什麼這個項目對你很重要？
## Part 6: Why Is This Project Important for You?

### 給UCLA的證明

當你在面試中打開這個項目說：

「我建立這個項目來證明我會用 Nilearn 提取腦連通性，用 NetworkX 找出樞紐區域。我可以比較患者和健康人，找出疾病的生物標誌物。

這正是 Dr Narr 的實驗室所做的。在UCLA，我會用這些技能分析你們的患者數據。然後，在第4-8週，我會學習量子計算，結合這些知識來加速中藥驗證。」

**這會讓他們印象深刻，因為：**
- ✅ 你已經會基本工具
- ✅ 你理解腦網絡的重要性
- ✅ 你知道怎樣連接到量子計算
- ✅ 你有具體的研究想法

---

## 第七部分：檢查清單 — 你準備好了嗎？
## Part 7: Checklist — Are You Ready?

在開始之前，確保你有：

- [ ] Python 已安裝
- [ ] 可以執行 `pip install nilearn networkx matplotlib pandas scipy`
- [ ] 了解什麼是 fMRI（4D腦掃描）
- [ ] 知道什麼是相關性（correlation）
- [ ] 願意花2.5小時完成這個項目

準備開始嗎？

