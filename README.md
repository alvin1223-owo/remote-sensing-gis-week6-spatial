# Week 6: Spatial Prediction Shootout - Kriging vs Machine Learning

## 專案概述

這個專案比較了四種不同的空間內插方法，將離散的降雨測站資料轉換為連續的降雨表面：

1. **Ordinary Kriging** (統計方法) - 使用空間相關性 + 提供不確定性估計
2. **Random Forest** (機器學習) - 使用資料模式 + 易於添加特徵
3. **Nearest Neighbor** (最近鄰居) - 簡單但產生馬賽克效果
4. **IDW** (反距離加權) - 產生牛眼效應

## 資料夾結構

```
week6/
├── Week6-Student.ipynb          # 主要分析筆記本
├── README.md                    # 本說明文件
├── .gitignore                   # Git 忽略檔案設定
└── data/                        # 資料檔案目錄
    ├── scenarios/                # 場景資料
    │   └── fungwong_202511.json # 颱風蕃薯降雨資料
    ├── riverpoly/               # 河流多邊形
    │   ├── riverpoly.shp        # 河流邊界 shapefile
    │   ├── riverpoly.dbf        # 屬性資料
    │   ├── riverpoly.prj        # 投影定義
    │   └── riverpoly.shx        # 索引檔案
    └── 鄉(縣、市、區)界線1140318/ # 台灣鄉鎮界線
        ├── TOWN_MOI_1140318.shp # 鄉鎮邊界 shapefile
        ├── TOWN_MOI_1140318.dbf # 屬性資料
        ├── TOWN_MOI_1140318.prj # 投影定義
        └── TOWN_MOI_1140318.shx # 索引檔案
```

## 主要檔案說明

### Week6-Student.ipynb
完整的分析筆記本，包含以下主要章節：

- **Cell [1]**: 環境設置與資料載入
- **Cell [2a-2d]**: 變異圖分析與參數優化
- **Cell [3]**: Kriging 內插執行
- **Cell [4-5]**: Random Forest 機器學習預測
- **Cell [6-8]**: 四種方法比較分析
- **Cell [9-9b]**: Kriging 不確定性分析 (Sigma Map)
- **Cell [10-11]**: GeoTIFF 匯出與分區統計

### 資料檔案

#### fungwong_202511.json
- **來源**: 中央氣象局 (CWA) 降雨觀測資料
- **事件**: 2025年颱風蕃薯事件
- **內容**: 各測站 1小時降雨量、坐標、時間戳記
- **格式**: CWA API JSON 格式

#### 河流與鄉鎮邊界
- **來源**: 內政部國土測繪中心 TGOS 資料
- **用途**: 空間參考與分區統計分析
- **坐標系**: TWD97 (EPSG:3826)

## 技術規格

- **坐標系統**: EPSG:3826 (TWD97)
- **網格解析度**: 1000m
- **研究區域**: 花蓮縣 + 宜蘭縣
- **時間點**: 單一時間點降雨分析

## 主要發現

1. **Kriging 優勢**: 提供不確定性估計，適合關鍵決策
2. **ML 限制**: 缺乏信心度量，但有特徵重要性分析
3. **傳統方法**: 產生明顯人工痕跡 (馬賽克、牛眼效應)
4. ** Nugget 效應**: 低 Nugget (1%) 更適合 CWA 校準測站

## 輸出成果

分析過程中產生的檔案 (未包含於 repo 中，因檔案過大):
- `interpolation_shootout.png` - 四種方法比較圖
- `kriging_vs_rf.png` - Kriging vs RF 直接比較
- `sigma_map.png` - Kriging 不確定性圖
- `kriging_rainfall.tif` - Kriging 降雨預測
- `kriging_variance.tif` - Kriging 方差圖
- `rf_rainfall.tif` - Random Forest 預測
- `township_rainfall_summary.csv` - 鄉鎮統計表

## 使用方法

1. 安裝必要套件:
```bash
pip install geopandas pandas numpy matplotlib pykrige scikit-learn scipy rasterio rasterstats
```

2. 執行筆記本:
```bash
jupyter notebook Week6-Student.ipynb
```

3. 按照筆記本中的指示逐步執行各個 cell

## 注意事項

- 大型檔案 (DEM、GeoTIFF、PNG) 已加入 .gitignore
- 如需完整資料集，請從原始資料來源下載
- 分析結果可能因隨機種子而略有差異

## 作者

Remote Sensing & GIS - Week 6 Spatial Prediction Analysis
