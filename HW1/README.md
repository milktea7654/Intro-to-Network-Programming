# Connect4 Network Game

**Score: 100/100** 

## 概述

這是一個基於 Python 的 Connect4 網路遊戲系統，包含大廳服務器和雙人遊戲模式。支持本地和遠端網路連線。

## 主要組件

- **大廳服務器** (`lobby_server.py`): 管理玩家註冊、登錄和配對
- **玩家客戶端** (`player_a.py`, `player_b.py`): 遊戲客戶端實現
- **遊戲邏輯** (`connect4.py`): Connect4 遊戲規則實現
- **共通模組** (`common.py`): 通訊工具和協議定義

## 快速開始

```bash
# 1. 啟動大廳服務器
python3 lobby_server.py --host 0.0.0.0 --port 12000

# 2. 啟動玩家 B (同一台機器)
python3 player_b.py --lobby-host 127.0.0.1 --lobby-port 12000 \
  --username testA --password 123 --udp-port 18000

# 3. 啟動玩家 A (遠端連線示例)
python3 player_a.py --lobby-host linux2.cs.nycu.edu.tw --lobby-port 12000 \
  --username milktea --password 7654 --scan-hosts linux2.cs.nycu.edu.tw \
  --scan-port-start 18000 --scan-port-end 18020
```

## 遊戲流程

1. **註冊**: 向大廳服務器註冊新帳戶
2. **登錄**: 使用用戶名和密碼登錄
3. **等待配對**: 服務器自動配對兩位玩家
4. **遊戲對戰**: UDP 通訊進行實時遊戲
5. **結束與統計**: 遊戲結束後顯示結果

## 技術細節

- **通訊協議**: UDP 點對點通訊
- **網路架構**: Client-Server 模型配合 UDP 遊戲通訊
- **併發處理**: 多線程支持多個玩家
- **身份驗證**: Username/Password 基礎認證

## 系統檢查

```bash
# 檢查運行中的服務器
ps aux | grep "lobby_server"

# 檢查端口佔用
netstat -tulpn | grep -E "12000"
```

## 故障排除

- **連線失敗**: 確認大廳服務器運行且端口正確
- **UDP 掃描失敗**: 檢查防火牆設置和掃描端口範圍
- **清除 Python 快取**: `find . -type d -name __pycache__ -exec rm -r {} +`
