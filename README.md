# Broker Trade Reconciliation App

Automated reconciliation tool for matching clearing broker trades with executing broker trades.

## Features

- **Multi-broker support**: ICICI, Kotak, IIFL, Axis, Equirus, Edelweiss, Nuvama, Morgan Stanley
- **Automatic detection**: Auto-detects broker from file structure and broker code
- **Password decryption**: Handles password-protected Excel files (Aurigin2024, Aurigin2017)
- **Smart matching**: 6-field matching with 0.001% price tolerance
- **Detailed diagnostics**: Shows why trades didn't match
- **CP Code validation**: Hard reject on CP code mismatches
- **Account detection**: Auto-prefixes outputs with account name (AURIGIN, WAFRA)

## Deployment

### Local Deployment

1. Install dependencies:
```bash
pip install -r requirements_recon.txt
```

2. Run the app:
```bash
streamlit run streamlit_recon.py
```

### Streamlit Cloud Deployment

1. Create a GitHub repository
2. Upload these files:
   - `streamlit_recon.py`
   - `trade_reconciliation.py`
   - `broker_parser.py`
   - `broker_config.py`
   - `futures mapping.csv`
   - `requirements_recon.txt` (rename to `requirements.txt`)
   - `.gitignore_recon` (rename to `.gitignore`)

3. Go to [share.streamlit.io](https://share.streamlit.io)
4. Connect your GitHub repo
5. Set main file path: `streamlit_recon.py`
6. Deploy!

## Usage

1. **Upload Clearing File**: Excel file from clearing broker
2. **Upload Futures Mapping**: Use default or custom CSV
3. **Upload Broker Files**: One or more broker files (all brokers supported)
4. **Run Reconciliation**: Click the button

## Outputs

1. **Enhanced Clearing File** (CSV):
   - Original clearing columns + 3 new columns:
   - `Comms`: Pure Brokerage Amount
   - `Taxes`: Total Taxes (GST + STT + Stamp Duty)
   - `TD`: Trade Date from broker file

2. **Reconciliation Report** (Excel with 4 sheets):
   - **Sheet 1**: Matched Trades
   - **Sheet 2**: Unmatched Clearing (with diagnostic columns)
   - **Sheet 3**: Unmatched Broker (with diagnostic columns)
   - **Sheet 4**: Summary Statistics

## Matching Criteria

Trades match on all 6 fields:
1. Bloomberg Ticker (normalized)
2. CP Code (case-insensitive)
3. Broker Code = TM Code (absolute value)
4. Buy/Sell (normalized)
5. Quantity (exact)
6. Price (0.001% tolerance)

## Supported Brokers

| Broker | Code | Auto-Detection |
|--------|------|----------------|
| ICICI Securities | 7730 | Broker Code column |
| Kotak Securities | 8081 | Column structure |
| IIFL Securities | 10975 | Broker Code or structure |
| Axis Securities | 13872 | Broker Code or structure |
| Equirus Securities | 13017 | Broker Code or structure |
| Edelweiss Securities | 11933 | Broker Code or structure |
| Nuvama Securities | 11933 | Filename or structure |
| Morgan Stanley | 10542 | "morgan" keyword in file |

## Technical Notes

- All broker files can be processed together in one run
- Password-protected Excel files are automatically decrypted
- Account prefixes: ECASL0000094 → AURIGIN, CITI00007707 → WAFRA
- CP Code mismatch results in hard reject with detailed breakdown
