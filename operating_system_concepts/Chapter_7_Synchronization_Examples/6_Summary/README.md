# 6. Summary

- 프로세스 동기화의 고전적인 문제 : bounded-buffer, readers-writers, dining-philosophers
    - mutex lock, semaphore, monitor, condition variable 등의 도구를 사용하여 해결
- Window OS는 dispatcher object를 사용
- Linux는 atomict variable, spinlock, mutex lock 사용
- POSIX API는 mutex lock, semaphore, condition variable 지원
    - named, unnamed semaphore 지원
- Java는 synchronization을 위한 라이브러리, API 지원
- 임계영역 문제를 해결하기 위한 대안책 : OpenMP, functional programming, transactional memory
