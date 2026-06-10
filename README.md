[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112839&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** minhhai032000@gmail.com
**Student ID:** 2A202600713
**Name:** Đặng Minh Hải

---

## Mo ta

Bai lab xay dung mot ETL pipeline tu dong doc du lieu JSON, kiem tra chat luong, chuan hoa va luu ra CSV. Sau do chay stress test voi Agent AI de quan sat anh huong cua du lieu "sach" vs du lieu "rac" len chat luong ket qua.

- **ETL Pipeline:** Extract → Validate → Transform → Load
- **Observability:** Logging so record valid/dropped, timestamp `processed_at` tren tung ban ghi
- **Stress Test:** So sanh ket qua Agent khi dung clean data (`processed_data.csv`) vs garbage data (`garbage_data.csv`)

---

## Cach chay (How to Run)

### Prerequisites

```bash
pip install pandas pytest
```

### Chay ETL Pipeline

```bash
python solution.py
```

Ket qua: file `processed_data.csv` duoc tao ra voi cac ban ghi hop le.

### Tao Garbage Data

```bash
python generate_garbage.py
```

### Chay Agent Simulation (Stress Test)

```bash
# Clean data vs Garbage data comparison
python agent_simulation.py
```

- **Clean data:** Agent dung `processed_data.csv` (output ETL da qua validation)
- **Garbage data:** Agent dung `garbage_data.csv` (co duplicate ID, outlier, null, sai kieu)

### Chay Tests

```bash
python -m pytest tests/test_autograder.py -v
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── raw_data.json            # Du lieu nguon (5 records)
├── processed_data.csv       # Output cua pipeline (3 records hop le)
├── garbage_data.csv         # Du lieu "rac" cho stress test
├── generate_garbage.py      # Script tao garbage data
├── agent_simulation.py      # Agent RAG gia lap
├── experiment_report.md     # Bao cao stress test
├── tests/
│   └── test_autograder.py   # Unit tests tu dong (khong sua)
└── README.md                # File nay
```

---

## Ket qua

| Buoc | Chi tiet |
|------|----------|
| Raw records doc vao | 5 |
| Records bi loai (price <= 0 hoac category rong) | 2 |
| Records hop le sau validation | 3 |
| Records luu vao `processed_data.csv` | 3 |

**Records bi loai:**
- `Mystery Box` (price = -10): gia am
- `Phone` (price = 800, category = ""): category rong

**Stress Test:**
- Clean data → Agent tra loi dung: *"Laptop at $1200"*
- Garbage data → Agent tra loi sai: *"Nuclear Reactor at $999,999"* (outlier)
