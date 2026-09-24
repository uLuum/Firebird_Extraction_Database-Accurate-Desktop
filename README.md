# Accessing Firebird Database of Accurate Desktop V5

Prototype untuk mengakses dan mengekstraksi database Firebird (`.gdb`) dari **Accurate Desktop V5** menggunakan Python dan Google Colab sebagai bagian dari **pre-migration preparation menuju Odoo ERP**.

## 🎯 Background

Dalam proses ERP migration, data tidak selalu bisa langsung dipindahkan dari source system ke target ERP.

Source database pada project ini menggunakan format Firebird `.gdb` dengan karakteristik legacy environment, sehingga modern Python environment tidak dapat langsung mengaksesnya.

Daripada langsung membuat proses import, project ini berfokus pada:

**Understand → Investigate → Validate → Extract → Profile → Map**

Tujuannya adalah mengurangi uncertainty dan memahami technical requirements sebelum proses migrasi sebenarnya dimulai.

## 🔍 What I Did

- Identifikasi database metadata menggunakan Firebird tools (Local Environment: `gstat -h`)
- Validasi **ODS 11.1 / SQL Dialect 3**
- Compatibility testing dengan Firebird 2.1 & 2.5
- Troubleshooting Firebird server, authentication, dan client library
- Reconstruct compatible **Firebird 2.5.9 runtime** di Google Colab
- Menyediakan legacy dependencies seperti `libncurses5` dan `libtinfo5`
- Resolve dynamic linking untuk `libfbclient.so.2`
- Menjalankan Firebird Server di environment Colab
- Melakukan authentication & connection validation
- Schema discovery menggunakan Firebird system tables
- Selective data extraction menggunakan Python & Pandas
- Data profiling sebagai persiapan cleaning dan transformation
- Menyiapkan dasar **field mapping menuju Odoo ERP**

## 🏗️ Prototype Flow

```text
Accurate Desktop V5
        │
        ▼
Firebird .GDB
        │
        ▼
Database & Compatibility Analysis
        │
        ▼
Legacy Runtime Reconstruction
        │
        ▼
Authentication & Connection Validation
        │
        ▼
Schema Discovery
        │
        ▼
Selective Data Extraction
        │
        ▼
Data Profiling & Validation
        │
        ▼
Transformation & Odoo Mapping
```

## 🔐 Authentication Investigation

Salah satu bagian penting dari troubleshooting adalah memahami authentication layer pada legacy Firebird environment.

Beberapa hal yang divalidasi:
- Credential yang digunakan oleh Firebird installation
- User yang tersedia melalui Firebird Security Database
- Authentication menggunakan `gsec`
- Database access menggunakan `isql`
- Application-level connection menggunakan Python/FDB

Validasi dilakukan secara bertahap:
```
GSEC
  ↓
User & Authentication
  ↓
ISQL
  ↓
Database Access
  ↓
Python / FDB
  ↓
Application Access
```
