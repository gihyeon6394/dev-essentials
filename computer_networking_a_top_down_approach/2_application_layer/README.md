# Chapter 2 Application Layer

1. [Principles of Network Applications](1_principles_of_network_applications/README.md)
2. [The Web and HTTP](2_the_web_and_http/README.md)
3. [Electronic Mail in the Internet](3_electronic_mail_in_the_internet/README.md)
4. [DNS-The Internet's Directory Service](4_dns_the_internets_directory_service/README.md)
5. [Peer-to-Peer File Distribution](5_peer_to_peer_file_distribution/README.md)
6. [Video Streaming and Content Distribution Networks](6_video_streaming_and_content_distribution_networks/README.md)
7. [Socket Programming: Creating Network Applications](7_socket_programming_creating_network_applications/README.md)

---

- Client-server architecture
    - 많은 인터넷 application은 클라이언트-서버 아키텍처를 따름
    - HTTP, SMTP, POP3, DNS 등에서 사용
- application layer protocol
    - e.g. HTTP, SMTP/POP3, FTP, DNS
- P2P architecture
    - e.g. BitTorrent, Skype
    - 사용자간에 직접 연결하여 데이터 전송
- Video streaming : CDN으로 부하분산
- Socket programming
    - 소켓 API 사용하여 네트워크 애플리케이션 개발
    - TCP, UDP 소켓 프로그래밍
- TCP : 신뢰, 연결 지향 (3-way handshake)
- UDP : 비신뢰, 비연결 지향 (데이터그램 전송)
