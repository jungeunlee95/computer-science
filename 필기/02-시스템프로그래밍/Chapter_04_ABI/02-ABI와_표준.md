[toc]

# ABI와 표준

## :heavy_check_mark: C 라이브러리

- 유닉스 C 라이브러리 - libc
- 리눅스 C 라이브러리 - GNU libc - glibc(지립씨)
  - 시스템콜, 시스템콜 래퍼, 기본 응용프로그램 기능 포함





<hr>

## :heavy_check_mark: C 컴파일러


- 유닉스 C 컴파일러 - cc

- 리눅스 C 컴파일러 - GNU cc - gcc

- 우분투 리눅스에 gcc 설치

  ```
  sudo apt-get install gcc
  gcc --version
  gcc -o test.c test
  ```

  



<hr>

## :heavy_check_mark: ABI (Application Binary Interface)


- 응용 프로그램 바이너리 인터페이스
- 함수 실행 방식, 레지스터 활용, 시스템 콜 실행, 라이브러리 링크 방식 등
- ABI가 호환되면 재컴파일 없이 동작
- 컴파일러, 링커(라이브러리 링크), 툴체인 (컴파일러를 만드는 프로그램)에서 제공





<hr>

## :heavy_check_mark: POSIX


- 유닉스 시스템 프로그래밍 인터페이스 표준
- IEEE(Institute of Eletronic and Electronics Engineers)에서 표준화 시도
- 리차드 스톨만 (자유 소프트웨어 재단)이 POSIX를 표준안 이름으로 제안




