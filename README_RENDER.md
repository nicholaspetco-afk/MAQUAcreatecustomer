# Render 部署包

此資料夾已整合部署到 Render 所需的檔案與目錄。上傳到 GitHub 後，直接在 Render 建立 Web Service 並指向這個 repo。

## 目錄結構
- `api/index.py`：WSGI 入口，匯出 `app`。
- `maqua-members/`：Flask 主程式、templates、static、services。
- `新增優化/`：customer_builder 相關解析器。
- `新增商機/`：opportunity_builder 解析器。
- `requirements.txt`：主依賴，包含 gunicorn；會再引入 `maqua-members/requirements.txt`。
- `.env.example`：環境變數範本（請在 Render 後台填入實際值，不要上傳真實密鑰）。
- `.gitignore`：避免提交 `.env`、`__pycache__` 等。

## Render 設定
1) **Build Command**: `pip install -r requirements.txt`
2) **Start Command**: `gunicorn api.index:app --bind 0.0.0.0:$PORT`
3) **Environment**: 建議新增環境變數 `PYTHON_VERSION=3.11`
4) **Environment Variables**: 依 `.env.example` 在 Render 後台新增 Production/Preview 變數（不可上傳 `.env`）。

## 上傳到 GitHub（本機範例）
```bash
cd render上傳
git init
git add .
git commit -m "render bundle"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

## 在 Render 建立 Web Service
- Render Dashboard → New + → Web Service → 連結上面的 GitHub repo。
- Branch: `main`
- Build Command / Start Command 如上。
- 填完環境變數後部署。

## 本地快測
```bash
pip install -r requirements.txt
python maqua-members/app.py  # http://127.0.0.1:6025
```

## 注意
- `maqua-members/services/config.py` 內含靜態憑證/ID，請確保 repo 為私有或改用環境變數後再公開。
- `.env.example` 只放範例值，正式值請在 Render 後台設定。
