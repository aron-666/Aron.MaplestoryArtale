# CUDA & cuDNN 安裝指南 (For Microsoft.ML.OnnxRuntime.Gpu 1.23.2)

本文件說明如何為 `Microsoft.ML.OnnxRuntime.Gpu` **1.23.2** 版本配置正確的 NVIDIA 運行環境。

## 📋 版本需求總覽

| 元件 | 建議版本 | 備註 |
| :--- | :--- | :--- |
| **Microsoft.ML.OnnxRuntime.Gpu** | **1.23.2** | 專案目前使用的版本 |
| **CUDA Toolkit** | **12.x** | 建議使用 **12.2** 或 **12.3** |
| **cuDNN** | **9.x** | 必須下載支援 CUDA 12.x 的版本 |
| **NVIDIA Driver** | **>= 536.25** | 需支援 CUDA 12.2 的驅動程式 |
| **作業系統** | Windows 10/11 (64-bit) | |

---

## 🖥️ 硬體需求

*   **顯示卡 (GPU):** NVIDIA GeForce GTX 10 系列或更新型號 (建議)。
*   **Compute Capability:** 需支援 **5.0** 以上 (Maxwell 架構)，建議 **6.0** (Pascal 架構) 以上以獲得最佳效能。

---

## 📥 下載連結

請依序下載以下檔案：

1.  **NVIDIA 顯卡驅動程式 (Driver)**
    *   [前往下載頁面](https://www.nvidia.com/Download/index.aspx)
    *   建議下載最新的 **Game Ready Driver** 或 **Studio Driver**。

2.  **CUDA Toolkit 12.2 (或 12.3)**
    *   [前往下載頁面 (Archive)](https://developer.nvidia.com/cuda-toolkit-archive)
    *   選擇 `CUDA Toolkit 12.2.x`。
    *   Operating System: `Windows` -> `x86_64` -> `10` or `11` -> `exe (local)`。

3.  **cuDNN 9.x (for CUDA 12.x)**
    *   [前往下載頁面](https://developer.nvidia.com/rdp/cudnn-archive)
    *   選擇 `Download cuDNN v9.x.x (for CUDA 12.x)`。
    *   下載 `Local Installer for Windows (Zip)`。

---

## 🛠️ 安裝步驟

請務必依照以下順序進行安裝：

### 步驟 1: 更新顯卡驅動程式
1. 執行下載的 NVIDIA 驅動程式安裝檔。
2. 選擇「Express (建議)」安裝。
3. 安裝完成後，建議**重新啟動電腦**。

### 步驟 2: 安裝 CUDA Toolkit
1. 執行下載的 CUDA Installer (`.exe`)。
2. 選擇 **Custom (Advanced)** 自訂安裝。
3. **重要：** 確保勾選 `CUDA > Development` 和 `CUDA > Runtime`。
    * *提示：若您已在步驟 1 安裝最新驅動，可取消勾選 Installer 中的 "Driver" 元件，避免舊版覆蓋新版。*
4. 依指示完成安裝。
5. 預設安裝路徑通常為：`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.2`

### 步驟 3: 安裝 cuDNN
1. 解壓縮下載的 cuDNN `.zip` 檔案。
2. 開啟解壓縮後的資料夾，您會看到 `bin`, `include`, `lib` 等資料夾。
3. 將這些資料夾內的檔案**複製並覆蓋**到 CUDA 的安裝目錄中：
    *   來源：`解壓縮資料夾\`
    *   目標：`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.2\`
4. 系統詢問是否取代檔案時，請選擇 **是 (Yes)**。

### 步驟 4: 檢查環境變數 (Environment Variables)
安裝程式通常會自動設定，但建議手動確認：
1. 按 `Win + S` 搜尋 **"編輯系統環境變數"**。
2. 點擊 **"環境變數"**。
3. 在 **"系統變數"** 區域找到 `Path`，點擊 **"編輯"**。
4. 確認清單中包含以下路徑 (版本號視安裝而定)：
    *   `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.2\bin`
    *   `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.2\libnvvp`

### 步驟 5: 驗證安裝
開啟 **PowerShell** 或 **CMD**，輸入以下指令：

```powershell
nvcc --version
```

若顯示類似 `release 12.x, V12.x.x.x` 的訊息，即代表 CUDA 安裝成功。

---

## ⚠️ 常見問題 (Troubleshooting)

### 1. 找不到 `zlibwapi.dll`
cuDNN 9.x 依賴 `zlibwapi.dll`。通常包含在 cuDNN 的 `bin` 資料夾中。
*   **解法：** 確保 `zlibwapi.dll` 位於 `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.2\bin` 目錄下（該目錄已在環境變數 Path 中）。
*   若程式執行仍報錯，可嘗試將 `zlibwapi.dll` 直接複製到程式的執行檔目錄 (`bin\Debug\net10.0-windows...` 或 `bin\Release\...`)。

### 2. 程式執行時報錯 `DllNotFoundException: onnxruntime_providers_cuda.dll`
*   **原因：** 系統找不到 CUDA 或 cuDNN 的 DLL 檔。
*   **解法：**
    1. 確認步驟 3 (複製 cuDNN 檔案) 是否正確執行。
    2. 確認步驟 4 (環境變數) 是否生效 (修改環境變數後需重啟 VS Code 或 Terminal)。
    3. 確認安裝的 CUDA/cuDNN 版本是否與 `Microsoft.ML.OnnxRuntime.Gpu` 1.23.2 匹配 (CUDA 12.x + cuDNN 9.x)。
