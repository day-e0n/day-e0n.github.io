---
title: "FTL 기초 - Flash Translation Layer란?"
date: 2025-11-09 12:00:00 +0900
categories: [FTL]
tags: [플래시메모리, SSD, 스토리지]
---

## FTL (Flash Translation Layer)

FTL은 플래시 메모리의 물리적 특성을 숨기고, 블록 디바이스처럼 사용할 수 있게 해주는 소프트웨어 계층입니다.

### 플래시 메모리의 특성

1. **In-place Update 불가능**
   - 데이터를 덮어쓰기 전에 반드시 삭제(erase)가 필요합니다
   - Erase 단위는 Write 단위보다 훨씬 큽니다

2. **제한된 P/E Cycle**
   - Program/Erase 횟수에 제한이 있습니다
   - 일반적으로 SLC: 100K, MLC: 10K, TLC: 3K 사이클

3. **연산 단위**
   - Read/Write: Page 단위 (4KB ~ 16KB)
   - Erase: Block 단위 (256KB ~ 4MB)

### FTL의 주요 기능

#### 1. 주소 매핑 (Address Mapping)

```
Logical Page Number (LPN) → Physical Page Number (PPN)
```

**매핑 방식:**
- **Page-level Mapping**: 세밀한 제어, 높은 메모리 오버헤드
- **Block-level Mapping**: 낮은 메모리 오버헤드, 내부 단편화 발생
- **Hybrid Mapping**: 두 방식의 장점을 결합

#### 2. 가비지 컬렉션 (Garbage Collection)

```python
def garbage_collection(block):
    valid_pages = []
    
    # 유효한 페이지 수집
    for page in block:
        if is_valid(page):
            valid_pages.append(page)
    
    # 유효한 페이지를 새 블록으로 이동
    new_block = allocate_free_block()
    for page in valid_pages:
        copy_page(page, new_block)
    
    # 기존 블록 삭제
    erase_block(block)
    return new_block
```

#### 3. 마모도 평준화 (Wear Leveling)

모든 블록이 골고루 사용되도록 하여 SSD의 수명을 연장합니다.

### 성능 최적화

- **Over-provisioning**: 여유 공간 확보로 GC 성능 향상
- **Write Buffer**: 쓰기 요청을 버퍼링하여 효율적으로 처리
- **Read Cache**: 자주 읽는 데이터를 캐싱

FTL 설계는 SSD의 성능과 수명을 결정하는 핵심 요소입니다! 💾
