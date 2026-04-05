# Connect4 Network Game

**Score: 120/120** 

## 概述

這是一個基於 Python 的 Connect4 網路遊戲系統，包含大廳服務器和雙人遊戲模式。

## 主要組件

### HW1: Connect4 Basic Implementation
- **大廳服務器** (`lobby_server.py`): 管理玩家註冊、登錄和匹配
- **玩家客戶端** (`player_a.py`, `player_b.py`): 遊戲客戶端實現
- **遊戲邏輯** (`connect4.py`): Connect4 遊戲規則實現
- **共通模組** (`common.py`): 通訊工具和協議定義

### HW2: Game Server with Database & Tetris
- **數據庫服務器** (`db_server.py`): SQLite 數據庫管理
- **大廳服務器** (`lobby_server.py`): 玩家與房間管理
- **遊戲服務器** (`game_server.py`): Tetris 遊戲邏輯與管理
- **遊戲客戶端** (`game_client.py`): 玩家遊戲介面
- **通訊協議** (`protocol.py`): 長度前綴幀協議
- **測試系統** (`test_system.py`): 自動化測試工具

## 快速開始

### HW1 - Connect4

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

### HW2 - Tetris with Database

```bash
# 1. 啟動數據庫服務器
python3 db_server.py

# 2. 啟動大廳服務器
python3 lobby_server.py

# 3. 啟動玩家客戶端
python3 lobby_client.py linux2.cs.nycu.edu.tw 10002

# 4. 運行測試系統
python3 test_system.py

# 5. 管理數據庫
sqlite3 ~/HW2/game_database.db
> SELECT * FROM User;
> SELECT * FROM Room;
> SELECT * FROM GameLog;
```

### 玩家操作

| 按鍵 | 功能 |
|------|------|
| 左/右方向鍵 | 移動方塊 |
| 上方向鍵 / X | 順時針旋轉 |
| Z | 逆時針旋轉 |
| 下方向鍵 | 軟降 |
| 空白鍵 | 硬降 |
| C | Hold |
| ESC | 退出遊戲 |

### 遊戲流程

1. **註冊/登錄**: 使用用戶名和密碼
2. **創建房間**: 選擇房間名稱、公開/私密、最大玩家數
3. **加入遊戲**: 進入房間並與其他玩家競技
4. **查看統計**: 檢視個人遊戲統計數據

## 系統檢查

```bash
# 檢查運行中的服務器
ps aux | grep "server.py"

# 檢查端口佔用
netstat -tulpn | grep -E "10001|10002"
```

## 技術細節

- **通訊協議**: 長度前綴幀協議（4字節頭 + JSON 主體）
- **數據庫**: SQLite3（User、Room、GameLog 表）
- **網路架構**: Client-Server 模型
- **併發處理**: 多線程支持

## 故障排除

- **數據庫重置**: `rm ~/HW2/game_database.db`
- **清除 Python 快取**: `find . -type d -name __pycache__ -exec rm -r {} +`
- **端口佔用**: 更改端口或終止現有進程
