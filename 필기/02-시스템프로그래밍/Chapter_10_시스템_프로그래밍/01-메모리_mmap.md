[TOC]

# 시스템 프로그래밍 - 메모리와 mmap

## :heavy_check_mark: 동적 메모리 생성하기

- heap 영역에 생성 - `malloc` 함수

![image-20210312200236891](assets/image-20210312200236891.png)






## :heavy_check_mark: 파일 처리 성능 개선 기법 - 메모리에 파일 매핑

```c
#include <sys/mman.h>
void *mmap(void *start, size_t length, int prot, int flags, int fd, off_t offset)
```

- [start~offset] ~ [start+offset_length] 만큼의 물리 메모리 공간을 mapping 할 것을 요청
- 보통 start: NULL 또는 0 사용, offset: mapping 되기 원하는 물리 메모리 주소로 지정
- prot: 보호 모드 설정
  - PROT_READ (읽기 가능) 
  - PROT_WRITE (쓰기 가능)
  - PROT_EXEC (실행가능)
  - PROT_NONE (접근 불가)
- flags: 메모리 주소 공간 설정
  - MAP_SHARED (다른 프로세스와 공유 가능)
  - MAP_PRIVATE (프로세스 내에서만 사용 가능)
  - MAP_FIXED (지정된 주소로 공간 지정)
- fd: device file에 대한 file descriptor





## :heavy_check_mark: mmap 동작 방식으로 이해하는 실제 메모리 동작 

> 운영체제, 가상 메모리 이해를 기반으로 실제 활용 총정리
>
> 컴퓨터 공학 이해 없이는 mmap 동작을 이해하기 어렵다

1. mmap 실행 시, 가상 메모리 주소에 file 주소 매핑 **(가상메모리 이해)**
2. 해당 메모리 접근 시, **(요구 페이징, lazy allocation)**
   - 페이지 폴트 인터럽트 발생
   - OS에서 file data를 복사해서 물리 메모리 페이지에 넣어줌
3. 메모리 read시: 해당 물리 페이지 데이터를 읽으면 됨
4. 메모리 write시: 해당 물리 페이지 데이터 수정 후, 페이지 상태 flag중 dirty bit를 1로 수정
   - 해당 페이지가 수정이 됐는지 확인 flag
5. 파일 close시: 물리 페이지 데이터가 file에 업데이트 됨 (성능 개선)





## :heavy_check_mark: 파일, 메모리, 가상 메모리

### 장점

- read(), write() 시 반복적인 파일 접근 방지하여 성능 개선
- mapping된 영역은 파일 처리를 위한 `lseek()`을 사용하지 않고 간단한 포인터 조작으로 탐색 가능하다.
  - 그렇기 때문에 read(), write() 로 해당 영역을 접근할때보다 파일에서 해당 데이터 주소를 찾아가는 시간을 단축할 수 있다.

### 단점

- mmap 가상메모리 페이지를 사용할때 고정된 블록 사이즈를 이용하기 떄문에 페이지 사이즈 단위로 매핑이 된다. 
  - 페이지 사이즈 단위의 정수배가 아닌 경우, 한 페이지 정도의 공간 추가 할당 및 남은 공간을 0으로 채워주게 된다. 예를 들어 1byte만 매핑을 시켜도 남은 공간은 0으로 채워진 4KB가 매핑이 된다.



