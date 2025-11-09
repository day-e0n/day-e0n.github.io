---
title: "RocksDB 소개 - LSM Tree 기반 스토리지 엔진"
date: 2025-11-09 11:00:00 +0900
categories: [RocksDB]
tags: [데이터베이스, LSM-Tree, Key-Value Store]
---

## RocksDB란?

RocksDB는 Facebook(Meta)에서 개발한 고성능 임베디드 Key-Value 스토리지 엔진입니다.

### 주요 특징

- **LSM Tree 구조**: Write-optimized 설계
- **Persistent Storage**: 디스크 기반의 영구 저장
- **Multi-threading**: 병렬 처리 지원
- **Compression**: 다양한 압축 알고리즘 지원

### LSM Tree (Log-Structured Merge Tree)

```
Write Path:
┌─────────┐
│MemTable │ (메모리)
└────┬────┘
     │ Flush
┌────▼────┐
│  SST 0  │ (디스크 - Level 0)
└────┬────┘
     │ Compaction
┌────▼────┐
│  SST 1  │ (Level 1)
└────┬────┘
     │
    ...
```

### 기본 사용법

```cpp
#include <rocksdb/db.h>

int main() {
    rocksdb::DB* db;
    rocksdb::Options options;
    options.create_if_missing = true;
    
    // DB 열기
    rocksdb::Status status = rocksdb::DB::Open(options, "/tmp/testdb", &db);
    
    // Write
    db->Put(rocksdb::WriteOptions(), "key1", "value1");
    
    // Read
    std::string value;
    db->Get(rocksdb::ReadOptions(), "key1", &value);
    
    // Delete
    db->Delete(rocksdb::WriteOptions(), "key1");
    
    delete db;
    return 0;
}
```

### Compaction 전략

RocksDB는 여러 Compaction 전략을 제공합니다:

1. **Leveled Compaction** (기본값)
   - Write amplification을 줄임
   - 읽기 성능 최적화

2. **Universal Compaction**
   - Write throughput 최대화
   - 공간 증폭 발생 가능

3. **FIFO Compaction**
   - 오래된 데이터 자동 삭제
   - 시계열 데이터에 적합

### 성능 튜닝 팁

```cpp
options.write_buffer_size = 64 * 1024 * 1024;  // 64MB
options.max_write_buffer_number = 3;
options.target_file_size_base = 64 * 1024 * 1024;
options.max_background_jobs = 4;
options.compression = rocksdb::kLZ4Compression;
```

RocksDB는 고성능이 필요한 다양한 애플리케이션에서 활용되고 있습니다! 🚀
