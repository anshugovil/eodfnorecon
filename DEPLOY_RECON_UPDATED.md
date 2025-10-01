# Broker Reconciliation - Updated Deployment Guide

## Files to Upload (8 files)

1. ✅ `streamlit_recon.py` **(UPDATED - re-upload)**
2. ✅ `trade_reconciliation.py` **(UPDATED - re-upload)**
3. ✅ `broker_parser.py`
4. ✅ `broker_config.py`
5. ✅ `account_config.py`
6. ✅ `Trade_Parser.py`
7. ✅ `futures mapping.csv`
8. ✅ `requirements.txt`

## Changes Made

### 1. streamlit_recon.py
**Fixed:** Futures mapping file path detection for Streamlit Cloud
- Now tries multiple locations
- Shows clear error if file not found

### 2. trade_reconciliation.py
**Fixed:** Better error messages for parsing failures
- Shows which files failed and why
- Helps diagnose broker file parsing issues

### 3. requirements.txt
```txt
streamlit>=1.28.0
pandas>=2.0.0
numpy>=1.24.0
openpyxl>=3.1.0
msoffcrypto-tool>=5.0.0
```

## Deployment Steps

### 1. Update GitHub Repo

Replace these 2 files with updated versions:
```bash
git add streamlit_recon.py trade_reconciliation.py
git commit -m "Fix Streamlit Cloud deployment issues"
git push
```

### 2. Check Streamlit Cloud

After pushing:
1. Streamlit Cloud will auto-redeploy
2. Check if error message is now more detailed
3. Look for specific failure reason (e.g., "futures mapping.csv not found", "Could not detect broker type", etc.)

## Troubleshooting

### Error: "futures mapping.csv not found"
**Solution:** Verify file is uploaded to GitHub repo with exact name (case-sensitive)

### Error: "Could not detect broker type from file"
**Possible causes:**
- File is password-protected (should auto-decrypt with Aurigin2024/Aurigin2017)
- File format not recognized
- File is corrupted

**Solution:** Try uploading a different broker file or check file locally first

### Error: "No trades found in file"
**Possible causes:**
- File has wrong structure
- Header row not detected
- Required columns missing

**Solution:** Compare file structure with working local files

### Still Getting Errors?

The new error messages will tell you exactly what's failing. Common issues:

1. **msoffcrypto-tool not installed**
   - Check Streamlit Cloud logs
   - Verify requirements.txt was updated

2. **File encoding issues**
   - Try re-saving the file as UTF-8 CSV
   - Try Excel format instead of CSV

3. **Memory issues**
   - Large files might timeout on Streamlit Cloud free tier
   - Try with smaller test files first

## Testing Locally First

Before deploying, test locally with the updated files:
```bash
streamlit run streamlit_recon.py
```

This ensures the changes work on your machine before pushing to cloud.

## Contact/Support

If the detailed error message still doesn't help identify the issue, share:
1. The complete error message
2. Broker file name (without sensitive data)
3. Screenshot of Streamlit Cloud logs (Manage app → Logs)
