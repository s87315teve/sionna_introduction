# NVIDIA Sionna: An Open-Source Library for Research on Communication Systems

## 概述

Sionna是由NVIDIA開發的開源通訊函式庫，專為下一代無線通訊系統的研究與設計打造而成。它整合了通訊系統與深度學習模組，使研究人員能夠輕鬆設計、測試以及最佳化通訊系統。

## 核心特色

### 基於TensorFlow的架構

Sionna建立在TensorFlow之上，具有以下優勢：
- 原生GPU加速
- 平行化的模擬能力
- 與深度學習框架之整合

### Link Level通訊系統設計

Sionna支援完整通訊的建模與模擬：
- 訊號源生成
- 通道編碼/解碼
- 調變/解調
- 通道估測
- 性能評估

### 高自由度

- 模組化功能設計
- 支援使用者自定義模型



## 實際應用
### 基礎教學 : 簡易End-to-end通訊模擬
[Demo_sionna_basic](sample_code/Demo_sionna_basic.ipynb)

在基礎教學中將學習如何使用Sionna建立一個簡單的End-to-end通訊系統。從二進位的原始資料到訊號的Modulation、Channel、Demodulation，最後還原出二進位原始資料並計算其Bit error rate(BER)
<figure>
  <img src="fig/sionna_simulate_basic.png">
  <figcaption>圖1：簡易End-to-end通訊架構</figcaption>
</figure>

### 應用一：系統性能評估
[Demo_LDPC_vs_Polar](sample_code/Demo_LDPC_vs_Polar.ipynb)

在應用一中建立了一個包含Channel coding具有功能的通訊系統模擬架構(如圖2)，包含了從二進位的原始資料生成到Channel coding與Modulation，到經過Channel後的Demodulation以及Channel decoding，並在最後還原出二進位的原始資料，以及比較LDPC和Polar兩種編碼方式的BER測試結果(圖3)。
<figure>
  <img src="fig/sionna_simulate_basic_encode.png">
  <figcaption>圖2：簡易End-to-end通訊架構</figcaption>
</figure>


<figure>
  <img src="fig/ldpc_vs_polar.png">
  <figcaption>圖3：LDPC和Polar的BER測試結果</figcaption>
</figure>



### 應用二：Neural Receiver
[Demo_neural_receiver](sample_code/Demo_neural_receiver.ipynb)

在應用二中建立了由神經網路構成的訊號接收器，從Channel estimation到Demodulation的步驟全部由AI來計算，並和基於LS estimation+Equalization以及Perfect CSI+Equalization的方法來進行比較(如圖4)。比較結果如圖5。
<figure>
  <img src="fig/sionna_neural_vs_baseline.png">
  <figcaption>圖4：Neural receiver和Baseline的比較結果</figcaption>
</figure>

<figure>
  <img src="fig/neural_vs_baseline.png">
  <figcaption>圖5：Neural receiver和Baseline的比較結果</figcaption>
</figure>


### 應用三：Ray tracing模擬
[Demo_ray_tracing](sample_code/Demo_ray_tracing.ipynb)

在應用三中將實作基於交大校園地圖的Ray tracing模擬，包含了點對點的Ray tracing計算+圖像化(圖6)以及取得校園內的Radio map(圖7)。

<figure>
  <img src="fig/demo_ray_tracing.png">
  <figcaption>圖6：點對點的Ray tracing模擬結果</figcaption>
</figure>
<figure>
  <img src="fig/demo_radio_map.png">
  <figcaption>圖7：交大校園Radio map</figcaption>
</figure>


## Setup

### Windows WSL
* 到BIOS打開虛擬化
* 用系統管理員權限打開Powershell並安裝WSL
```
wsl --install
```
* 安裝WSL的Ubuntu22.04，去Microsoft Store下載 (之後Ubuntu的資料夾路徑會出現在檔案總管的Linux的位置)
* 按Windows鍵，搜尋Ubuntu並開啟，並參考以下Ubuntu的教學
### Ubuntu
* 打開Terminal並輸入以下指令
```
sudo apt update && sudo apt upgrade -y
sudo apt install build-essential
sudo apt install llvm
```
* 下載&安裝conda
```
wget https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
bash Anaconda3-2024.10-1-Linux-x86_64.sh
```
* 建立虛擬環境
```
conda create --name sionna_env python=3.9
conda activate sionna_env
```
* 安裝套件
```
pip install sionna==0.19.2
pip install tensorflow==2.14.0
pip install numpy==1.23.5
pip install jupyter notebook
conda install -c conda-forge libstdcxx-ng
```

#### Cuda環境設定(如果可以使用的話)
* 需先在Windows上安裝Nvidia driver(請到官網下載安裝)
* 安裝Cuda，以Cuda11.8為例 
```
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run --silent --toolkit
```
* 安裝Cudnn，以Cudnn11.x為例
```
wget https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/linux-x86_64/cudnn-linux-x86_64-8.9.7.29_cuda11-archive.tar.xz
tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda11-archive.tar.xz
sudo cp cudnn-linux-x86_64-8.9.7.29_cuda11-archive/include/cudnn* /usr/local/cuda-11.8/include
sudo cp cudnn-linux-x86_64-8.9.7.29_cuda11-archive/lib/libcudnn* /usr/local/cuda-11.8/lib64/
sudo chmod a+r /usr/local/cuda-11.8/include/cudnn* /usr/local/cuda-11.8/lib64/libcudnn*
```
* 設定Cuda路徑，在```~/.bashrc```裡面加上
```
export CUDA_HOME=/usr/local/cuda-11.8
export PATH=$PATH:$CUDA_HOME/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_HOME/lib64
```
* 在Terminal輸入
```
source ~/.bashrc
```

* (Optional)處理WSL中Mitsuba的GPU加速問題，參考[Mitsuba on WSL2](https://mitsuba.readthedocs.io/en/stable/src/optix_setup.html)

* 3D地圖建置教學可以參考 [3D map tutorial](3dmap_tutorial/README.md)


## References
* NVIDIA website: https://developer.nvidia.com/sionna
* Sionna tutorial: https://nvlabs.github.io/sionna/index.html
* Sionna GitHub: https://github.com/NVlabs/sionna
