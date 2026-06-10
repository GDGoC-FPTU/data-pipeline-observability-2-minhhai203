# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600713
**Name:** Đặng Minh Hải
**Date:** 2026-06-10

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200." | 9 | Correct product, valid price, proper category match |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Extreme outlier dominates result; answer is nonsensical |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi su dung garbage_data.csv, Agent tra lai ket qua hoan toan sai vi tap du lieu chua nhieu van de chat luong nghiem trong:

**1. Extreme Outlier (Gia tri ngoai le cuc doan):** Ban ghi "Nuclear Reactor" co gia $999,999 la mot outlier cuc doan so voi gia tri binh thuong cua san pham electronics (~$1,200). Vi agent su dung logic `idxmax()` de tim san pham co gia cao nhat, gia tri bat thuong nay nen hoat dong "chon san pham tot nhat" cho ket qua phi thuc te.

**2. Duplicate ID (ID trung lap):** Co 2 ban ghi co id=1 (Laptop va Banana). Du lieu bi nham lan giua cac danh muc hoan toan khac nhau, gay ra khong nhat quan trong knowledge base cua Agent.

**3. Wrong Data Type (Sai kieu du lieu):** Ban ghi "Broken Chair" co gia la chuoi "ten dollars" thay vi so. Dieu nay khien pandas khong the so sanh gia tri gia chinh xac, dan den ket qua khong the du doan truoc.

**4. Null Values (Gia tri null):** Ban ghi "Ghost Item" co id=None va category=None. Agent co the gap loi hoac xu ly sai khi gap gia tri null trong qua trinh truy van.

Tat ca nhung van de nay cho thay rang: du cho Agent co logic xu ly tot den dau, neu du lieu dau vao khong duoc kiem soat chat luong, ket qua dau ra se khong the tin cay duoc.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Dong y.

Du lieu sach la nen tang khong the thay the cua bat ky he thong AI nao. Trong thi nghiem nay, cung mot Agent logic, cung mot cau hoi, nhung chi can thay doi nguon du lieu tu clean sang garbage la ket qua thay doi hoan toan - tu chinh xac (Laptop $1,200) sang sai hoan toan (Nuclear Reactor $999,999). Mot prompt tot hay mot model manh khong the bu dap duoc khi du lieu dau vao da bi "doc" boi outlier, null values hay duplicate. Do do, ETL pipeline voi validation nghiem ngat la buoc bat buoc truoc khi bat ky AI Agent nao co the hoat dong dang tin cay.
