### Context
GCP VM에 n8n 셀프호스팅을 구축하던 중, DuckDNS 도메인(`chutzrit.duckdns.org`)을 이용하여 Caddy를 통해 Let's Encrypt로 HTTPS 인증서 발급을 시도했습니다. 그러나 인증서 발급 과정에서 반복적인 오류가 발생했습니다.

### Core
오류 메시지는 다음과 같습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Let's Encrypt는 인증서 발급 전 `duckdns.org`의 CAA 레코드를 조회하는 과정에서 DuckDNS 네임서버가 불안정하게 동작하여 SERVFAIL이 발생했습니다. ZeroSSL을 사용해도 동일한 문제가 발생했으며, 이는 DNS 문제 때문임을 알게 되었습니다.

VM의 DNS 서버를 `1.1.1.1`로 변경하는 방법으로 일시적으로 문제를 해결할 수 있었으나, 근본적인 해결책은 아니었습니다. 결국 Caddy를 포기하고 Ngrok으로 전환하여 문제를 해결했습니다.

### Insight
DuckDNS는 무료 DDNS 서비스로 네임서버의 안정성이 부족할 수 있습니다. 이는 개인적인 테스트 목적에는 적합하나, 가용성과 재현성이 중요한 환경에서는 제약이 있을 수 있습니다. 반면, Ngrok은 도메인, DNS, SSL 문제를 동시에 해결하고 VM 방화벽 설정 없이도 쉽게 셀프호스팅을 구성할 수 있어 초보자에게 훨씬 적합한 솔루션입니다. 

**출처:** [DuckDNS GitHub Issues](https://github.com/duckdns/duckdns/issues), [Caddy Documentation](https://caddyserver.com/docs/)